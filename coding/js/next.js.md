# Pages Router

## [Project structure](https://nextjs.org/docs/getting-started/project-structure)

```
# special files
_app -> client side, page overrides, modifies all pages
_document -> server side, metadata, modifies html root
404 -> not found page fallback
500 -> internal server error page fallback

# routing files
index -> root page
file -> page relative to root, i.e; /foo
folder/index -> alternative page relative to root
folder/file -> page relative to folder, i.e; /foo/bar
pages/api/* -> create an API endpoint
```

## [Pages and Layouts](https://nextjs.org/docs/pages/building-your-application/routing/pages-and-layouts)
 single shared layout with custom app
```tsx
import Layout from '../components/layout'
 
export default function App({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  )
}
```

per-page layouts
```tsx
import Layout from '../components/layout'
import NestedLayout from '../components/nested-layout'
 
export default function Page() {
  return (
    /** Your content */
  )
}
 
Page.getLayout = function getLayout(page) {
  return (
    <Layout>
      <NestedLayout>{page}</NestedLayout>
    </Layout>
  )
}
```

## [Dynamic Routes](https://nextjs.org/docs/pages/building-your-application/routing/dynamic-routes)

convention
```
[dynamic]/foo.tsx -> abc/foo, def/foo will lead to the same page
foo/[dynamic].tsx -> foo/abc, foo/def will lead to the same page
```

catch-all
```
foo/[...dynamic].tsx -> foo/abc, foo/abc/def will lead to the same page
```

optional catch-all
```
foo/[[...dynamic]].tsx -> foo, foo/abc, foo/abc/def will lead to the same page
```

useRouter
```tsx
import {useRouter} from 'next/router'

// example [dynamic]/foo
export default function Foo() {
	// path in the url is abc/foo
	const router = useRouter()
	return <h1>{router.query.dynamic}</h1> // returns abc
}

// example foo/[...dynamic]
export default function Bar() {
	// path in the url is foo/abc/def
	const router = useRouter()
	return <h1>{router.query.dynamic}</h1> // returns ['abc', 'def']
}
```

## [Api Routes](https://nextjs.org/docs/pages/building-your-application/routing/api-routes)

basic
```ts
import type { NextApiRequest, NextApiResponse } from 'next'
 
type ResponseData = {
  message: string
}
 
export default function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseData>
) {
  res.status(200).json({ message: 'Hello from Next.js!' })
}
```

handling http methods
```ts
import type { NextApiRequest, NextApiResponse } from 'next'
 
export default function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === 'POST') {
    // Process a POST request
  } else {
    // Handle any other HTTP method
  }
}
```