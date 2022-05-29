

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

**Pre-rendering is generating HTML with its content in the server prior to send it to the client.**
  - static generation
  - server side rendering

## static generation

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
   
- 

