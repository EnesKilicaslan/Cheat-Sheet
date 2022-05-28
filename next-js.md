

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

### static generation

The HTML is generated at build time and is reused on each request. Static generation can be done with or without data.
Page can be built once, cached by a CDN and served almost instantly.
Blog pages, e commerce product pages, documentations and marketing pages are good candidate to be statically rendered.

`getStaticProps` is used to fetch data at build time for static generation

- pages is a special folder and `getStaticProps` is only available for the files under this folder. This is where a component and a page differenciate.

any link component in the viewport will be pre-fetched by default for pages using static generation.
When navigating to the link component from another compoennt, the page is generated on client using javascript and pre-fetched JSON data.
If you navigate to the page directly, html page is fetched.

`getStaticPaths` is required for dynamic static site generation
