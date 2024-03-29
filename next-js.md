
## ROUTING

## catch-all routes

filename should be [...params].js to capture all sub-routes

## navigation
Link component should be used as following for client side navigation

<Link href="" >
  <a>...</a>
</Link>

replace prop will replace history instead of adding

we can navigate to a page programatically by using push method of router hook

```
router.push("/")

router.replace("/") // replace history
```

## PRE-RENDERING

**Pre-rendering is generating HTML with its content in the server prior to send it to the client.**
  - static generation
  - server side rendering

## static generation

If we want to render 404 page, we can return {notFound: true} in the getStaticProps or getServerSideProps functions.

### Static Site Generation

The HTML is generated at build time and is reused on each request. Static generation can be done with or without data.
Page can be built once, cached by a CDN and served almost instantly.
Blog pages, e commerce product pages, documentations and marketing pages are good candidate to be statically rendered.

`getStaticProps` is used to fetch data at build time for static generation

- pages is a special folder and `getStaticProps` is only available for the files under this folder. This is where a component and a page differenciate.
- getStaticProps should return object that contains props key which is an object.
- getStaticProps runs once at build time in prod, howeever it runs for each request in dev mode.


any link component in the viewport will be pre-fetched by default for pages using static generation. So, on navigation no need to make an additional request
When navigating to the link component from another compoennt, the page is generated on client side using javascript and pre-fetched JSON data.
If you navigate to the page directly, html page is fetched.

*when we have a child  that does not have a as child in the link component, we pass passHref prop*

- `getStaticPaths` is required for static site generation with  dynamic path pages, for example: "posts/[postId]"
- `getStaticPaths` is used to inform Next.JS what are the possible values for the dynamic path would be.
- It returns an object which contains a key paths which itself is an array of objects

for example:
```

export async function getStaticPaths() {
  return {
    paths: [
      { params: { postId: '1' } },
      { params: { postId: '2' } },
      { params: { postId: '3' } },
      { params: { postId: '4' } },
      { params: { postId: '5' } }
    ],
    fallback: false
  }
}
```

- `fallback: false` means display 404 page if the path is not in the list of `getStaticPaths`
- `fallback: true` don't display 404 page. instead it will serve a fallback version of the page by running `getStaticProps` which generates html and json for the path.
   Next.js keeps track of new rendered pre-rendered list of pages and subsequent call will result the same page 
- `fallback: blocking` is really the same `true` except from first page load. The pages that have not been generated at build time don't display 404 page as in the case of `true`. however the page does not return immediatly the code block for `isFallback` (i.e loading indicator ) but generates the page on server and returns the whole page.
- `fallback: blocking` is mainly created for the crawlers that does not support Javascript. 


#### pros
- pre-rendered static pages can be pushed to a CDN and served to clients across glob almost instantly
- static content is better for SEO

#### cons
- the build time is proportional to the number of pages
- a page, once generated, can contain stale data  

**ISR (Incremental Static Rendering)** solves stale data problem without re-building the entire app

**ISR** can be applied to a page by specifying the **revalidate** key (which is in seconds) in the `getStaticProps` function


### Server Side Rendering

- pre-render a page not at build time but at request time, HTML is generated for every incoming request. ( it is usefull for fetching personalized data and keeping in mind SEO ) 
- `getServerSideProps` function is used for server side rendering. it returns an object that contains `props` key which is again an object
- `getServerSideProps` never runs on client side.
- `getServerSideProps` has access to request, response and query objects unlike `getStaticProps`

Next.JS also allows for client side data fetching. For private (behind login screen), highly user specific and no SEO required pages client side data fetching is more suitable.
Client side data fetching is exactly the same as React.JS, we make the `fetch` request in `useEffect` hook.
`SWR` is a hook for data fetching suggested by Next.JS 

- We can combine SSR and client side fetching ( for example, we can use client side fetching for filtering/searching) and shallow routing can be used to change the url on the browser.

##### sitemap.xml generation tutorial
https://cheatcode.co/tutorials/how-to-generate-a-dynamic-sitemap-with-next-js


## API ROUTES 

Next.JS is fullstack framework. we can create api folder within the pages folder.

we can access the endpoint by navigating the /api URL with the correct file structure.

in the file there should be a default exported function

```request.method``` is used to identify the type of method: (such as GET, POST, DELETE)

catch all routes: [...params]
catch all routes with optional params: [[...params]]

calling local api in ``getStaticProps`` or ``getServerSideProps`` is not recommended as it will create another round trip. Instead, we can replicate the same logic within getStaticProps ot getServerSideProps itself. ( We should do code repetition, so we can write the logic in a seperate file and export it)


## STYLING

#### global styles

We can manually edit the ``global.css`` file or we can create another styling file and import it right after the global style in the ``_app.js`` file.
On the other hand, we can use styles from external libraries by importing it in the ``_app.js`` file.

#### component level styles

It is alse called as Module CSS. We can create a file that starts with the component name and ends with `.module.css`. This way we can directly access the classes as they are JS objects and we can easily avoid name collision with this way.

### sass support

instead of css, we can use scss with Next's default features. First install sass ``yarn add sass`` and name the files with extention ``.scss``.


## MISCELLANEOUS

### App Layout

We can create common components and include them in the App component. For example, Header and Footer components. If there is a page that we don't want those pages, we can specify `getLayout` property with `PageLayout`.

### Head Component

<head> tag is good for SEO. We can use Next's **Head** component to get benefit from <head> tag. We can put <title> and <meta> tags inside of the **Head** Component.
Next.JS merge and overrides the content of the **Head** component in the App component.

### Image Component
  - can optimize the size
  - lazy loading
  - placeholder image, blur version: prevents UI shift

### Absolute Imports
  we can use absolute paths istead of specifiying relative paths(with lots of '..').
  in order to do that, we can create ``jsconfig.json`` file and put the following according to the needs
  
  ```javascript
  {
    'compilerOptions': {
        'baseUrl': '.',
        'paths': {
          '@name/layout': ['components/layout/*']  
        }
    }
  } 
  ```
  
### Typescript Support
  - create a file named `tsconfig.json`
  - `yarn add typescript @types/react`
  - restart the dev server and it will populate the `tsconfig.json` file
  - Next.JS also creates `next-env.d.ts` typings file
  - When we do this, `jsconfig.json` is not in control anymore, `tsconfig.json` is in control.
  - `GetStaticProps`, `GetStaticPaths`, `GetServerSideProps`, `NextApiRequest`, `NextApiResponse` are types of Next.JS  
  
### Redirects
  - Redirects are specificied in the `next.config.js` file as following.
  
  ```javascript
  module.exports = {
  redirects: async () => {
    return [
      {
        source: '/about',
        destination: '/',
        permanent: false
      }
    ]
  }
  }
  ```
  
  - permanent means the page permanently redirected (possibly because of movement of the files) and status code ise set to 308 automatically by Next.JS
  - if we set permanent to false, it means page will be redirected temporarily possibly due to maintainance.
  
### Enviroment Variables
  - apps typically require enviroment variables.
  - they allow use to define values based on development, production and testing.
  - enables safeguard secrets by not bundling them into the code that is shipped to the browser
  - the file name has to be `.env.local`.
  - we can access the variables defined in the file `.env.local` from `getStaticProps` and `getServerSideProps` by using `process.env.[variable-name]`
  - as note, we can not destructure `proccess.env`
  - in the component jsx we can not access the enviroment variables, meaing they are not exposed to the browser
  - to expose the enviroment variables to public we need to add `NEXT_PUBLIC_` prefix to the variable.
  - Enviroment variables are used to set base url, secrets like API-KEY
  - example `.env.local` file is as following;
  
  ```
    API_KEY=EF2JSA@!ASDSAD$#
    DB_USER_PASS=next!paswds
    NEXT_PUBLIC_ANALYTICS_ID=1234
  ```
  
  

