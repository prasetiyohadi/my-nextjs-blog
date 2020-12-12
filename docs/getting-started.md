# Getting started

## Initialize a Next.js app

* Create next.js app from template using npm
```
$ npx create-next-app my-nextjs-blog --use-npm --example https://github.com/vercel/next-learn-starter/tree/master/learn-starter

Success! Created my-nextjs-blog at $PWD/my-nextjs-blog
Inside that directory, you can run several commands:

  npm run dev
    Starts the development server.

  npm run build
    Builds the app for production.

  npm start
    Runs the built app in production mode.

We suggest that you begin by typing:

  cd my-nextjs-blog
  npm run dev
```
* Edit ``pages/index.js`` with your text editor and change **"Welcome to"** to **"Learn"**
* Next.js development server has [Fast Refresh](https://nextjs.org/docs/basic-features/fast-refresh) enabled and automatically applies the changes from files to the browser almost instantly

## Navigate between pages

* Create directory ``pages/posts`` and file ``pages/posts/first-post.js`` with the following content to create new page using JSX and React Components
```
export default function FirstPost() {
  return <h1>First Post</h1>
}
```
* Use ``Link`` component form ``next/link`` to enable **client-side navigation** between two pages in the index page ``pages/index.js``
```
import Link from 'next/link'

# code

<h1 className="title">
  Read{' '}
  <Link href="/posts/first-post">
    <a>this page!</a>
  </Link>
</h1>
```
* Add link to go back index page in ``pages/posts/first-post.js``
```
import Link from 'next/link'

export default function FirstPost() {
  return (
    <>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
```
* Next.js does code splitting automatically so each page only loads what's necessary for that page
* Whenever ``Link`` components appear in the browser's viewport Next.js automatically **prefetches** the code for the linked page in the background

## Assets, Metadata, CSS

* Next.js can serve static files or assets under **the top-level** ``public`` directory that can be referenced from the root of application
* Add ``Head`` components to ``first-post.js`` so we can add metadata to the application
```
import Link from 'next/link'
import Head from 'next/head'

export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
```
* Create **Layout** component in next.js by creating a top-level directory ``components`` and file ``components/layout.js`` with the following content
```
export default function Layout({ children }) {
  return <div>{children}</div>
}
```
* Update ``pages/posts/first-post.js`` and import ``Layout`` component and make it the outermost component
```
import Head from 'next/head'
import Link from 'next/link'
import Layout from '../../components/layout'

export default function FirstPost() {
  return (
    <Layout>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </Layout>
  )
}
```
* Add some styles using **CSS styling** to the ``Layout`` component using [CSS Modules](https://nextjs.org/docs/basic-features/built-in-css-support#adding-component-level-css) by creating file ``components/layout.module.css`` with the following content
```
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
```
* Import the CSS file with a name like ``styles`` and use it in the code in file ``components/layout.js``
```
import styles from './layout.module.css'

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
```
* To load global CSS files, create a file called ``pages/_app.js`` with the following content then restart the development server
```
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```
* Global CSS can only be imported from ``pages/_app.js`` but we can put the CSS file anywhere for example ``styles/global.css`` with the following content
```
html,
body {
  padding: 0;
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen, Ubuntu,
    Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  line-height: 1.6;
  font-size: 18px;
}

* {
  box-sizing: border-box;
}

a {
  color: #0070f3;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

img {
  max-width: 100%;
  display: block;
}
```
* Import the CSS file in the ``pages/_app.js`` file and make sure to restart the development server
```
import '../styles/global.css'

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```
* Create directory ``public/images`` and save your ``profile.jpg`` there
* Update ``components/layout.module.css`` with the following content
```
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}

.header {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.headerImage {
  width: 6rem;
  height: 6rem;
}

.headerHomeImage {
  width: 8rem;
  height: 8rem;
}

.backToHome {
  margin: 3rem 0 0;
}
```
* Create ``styles/utils.module.css`` for typography and others that will be useful across multiple components
```
.heading2Xl {
  font-size: 2.5rem;
  line-height: 1.2;
  font-weight: 800;
  letter-spacing: -0.05rem;
  margin: 1rem 0;
}

.headingXl {
  font-size: 2rem;
  line-height: 1.3;
  font-weight: 800;
  letter-spacing: -0.05rem;
  margin: 1rem 0;
}

.headingLg {
  font-size: 1.5rem;
  line-height: 1.4;
  margin: 1rem 0;
}

.headingMd {
  font-size: 1.2rem;
  line-height: 1.5;
}

.borderCircle {
  border-radius: 9999px;
}

.colorInherit {
  color: inherit;
}

.padding1px {
  padding-top: 1px;
}

.list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.listItem {
  margin: 0 0 1.25rem;
}

.lightText {
  color: #999;
}
```
* Update ``components/layout.js`` to also import the new CSS module and add new component with the following content
```
import Head from 'next/head'
import styles from './layout.module.css'
import utilStyles from '../styles/utils.module.css'
import Link from 'next/link'

const name = 'Your Name'
export const siteTitle = 'Next.js Sample Website'

export default function Layout({ children, home }) {
  return (
    <div className={styles.container}>
      <Head>
        <link rel="icon" href="/favicon.ico" />
        <meta
          name="description"
          content="Learn how to build a personal website using Next.js"
        />
        <meta
          property="og:image"
          content={`https://og-image.now.sh/${encodeURI(
            siteTitle
          )}.png?theme=light&md=0&fontSize=75px&images=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Ffront%2Fassets%2Fdesign%2Fnextjs-black-logo.svg`}
        />
        <meta name="og:title" content={siteTitle} />
        <meta name="twitter:card" content="summary_large_image" />
      </Head>
      <header className={styles.header}>
        {home ? (
          <>
            <img
              src="/images/profile.jpg"
              className={`${styles.headerHomeImage} ${utilStyles.borderCircle}`}
              alt={name}
            />
            <h1 className={utilStyles.heading2Xl}>{name}</h1>
          </>
        ) : (
          <>
            <Link href="/">
              <a>
                <img
                  src="/images/profile.jpg"
                  className={`${styles.headerImage} ${utilStyles.borderCircle}`}
                  alt={name}
                />
              </a>
            </Link>
            <h2 className={utilStyles.headingLg}>
              <Link href="/">
                <a className={utilStyles.colorInherit}>{name}</a>
              </Link>
            </h2>
          </>
        )}
      </header>
      <main>{children}</main>
      {!home && (
        <div className={styles.backToHome}>
          <Link href="/">
            <a>← Back to home</a>
          </Link>
        </div>
      )}
    </div>
  )
}
```
* Update ``pages/index.js`` and use the new CSS module
```
import Head from 'next/head'
import Layout, { siteTitle } from '../components/layout'
import utilStyles from '../styles/utils.module.css'

export default function Home() {
  return (
    <Layout home>
      <Head>
        <title>{siteTitle}</title>
      </Head>
      <section className={utilStyles.headingMd}>
        <p>[Your Self Introduction]</p>
        <p>
          (This is a sample website - you’ll be building a site like this on{' '}
          <a href="https://nextjs.org/learn">our Next.js tutorial</a>.)
        </p>
      </section>
    </Layout>
  )
}
```
* Use ``classnames`` library to toggle classes and check its [documentation](https://github.com/JedWatson/classnames)
```
$ npm install classnames
```
```
// alert.module.css
.success {
  color: green;
}
.error {
  color: red;
}
```
```
// alert.js
import styles from './alert.module.css'
import cn from 'classnames'

export default function Alert({ children, type }) {
  return (
    <div
      className={cn({
        [styles.success]: type === 'success',
        [styles.error]: type === 'error'
      })}
    >
      {children}
    </div>
  )
}
```
* Customize Next.js compiler [PostCSS](https://postcss.org/) by creating a top-level file called ``postcss.config.js`` which is useful if you're using libraries like [Tailwind CSS](https://tailwindcss.com/)
```
$ npm install tailwindcss postcss-preset-env postcss-flexbugs-fixes
```
```
// postcss.config.js
module.exports = {
  plugins: [
    'tailwindcss',
    'postcss-flexbugs-fixes',
    [
      'postcss-preset-env',
      {
        autoprefixer: {
          flexbox: 'no-2009'
        },
        stage: 3,
        features: {
          'custom-properties': false
        }
      }
    ]
  ]
}
```
```
// tailwind.config.js
module.exports = {
  purge: [
    // Use *.tsx if using TypeScript
    './pages/**/*.js',
    './components/**/*.js'
  ]
  // ...
}
```
* Import [Sass](https://nextjs.org/docs/basic-features/built-in-css-support#sass-support) into Next.js using both ``.scss`` and ``.sass`` extensions and use component-level Sass via [CSS Modules](https://nextjs.org/docs/basic-features/built-in-css-support#adding-component-level-css) and the ``.module.scss`` or ``.module.sass`` extension
```
$ npm install sass
```

## Pre-rendering and Data Fetching

* [Pre-rendering](https://nextjs.org/docs/basic-features/pages#pre-rendering) is the most important concepts in Next.js which Next.js pre-renders every page to HTML with its minimal JavaScript code necessary for each page
* **Hydration** is a process when the JavaScript code is run when a page is loaded by the browser
* There are two forms of pre-rendering: [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) and [Server-side Rendering](https://nextjs.org/docs/basic-features/pages#server-side-rendering)
  * [Static Generation](https://nextjs.org/docs/basic-features/pages#static-generation-recommended) generates HTML at **build time**
  * [Server-side Rendering](https://nextjs.org/docs/basic-features/pages#server-side-rendering) generates HTML on **each request**
* Static Generation is recommended whenever possible because your page can be built once and served by CDN which makes it much faster than having a server render the page on every request
* Static generation can be done **with** and **without data** and we can use an ``async`` function called ``getStaticProps`` to fetch external data and send it as props to the page
```
export default function Home(props) { ... }

export async function getStaticProps() {
  // Get external data from the file system, API, DB, etc.
  const data = ...

  // The value of the `props` key will be
  //  passed to the `Home` component
  return {
    props: ...
  }
}
```
* Create a top-level directory to save our blog data called **posts** and create two files ``posts/pre-rendering.md`` and ``posts/ssg-ssr.md``
  * The content of ``posts/pre-rendering.md``
  ```
  ---
  title: 'Two Forms of Pre-rendering'
  date: '2020-01-01'
  ---

  Next.js has two forms of pre-rendering: **Static Generation** and **Server-side Rendering**. The difference is in **when** it generates the HTML for a page.

  - **Static Generation** is the pre-rendering method that generates the HTML at **build time**. The pre-rendered HTML is then _reused_ on each request.
  - **Server-side Rendering** is the pre-rendering method that generates the HTML on **each request**.

  Importantly, Next.js lets you **choose** which pre-rendering form to use for each page. You can create a "hybrid" Next.js app by using Static Generation for most pages and using Server-side Rendering for others.
  ```
  * The content of ``posts/ssg-ssr.md``
  ```
  ---
  title: 'When to Use Static Generation v.s. Server-side Rendering'
  date: '2020-01-02'
  ---

  We recommend using **Static Generation** (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

  You can use Static Generation for many types of pages, including:

  - Marketing pages
  - Blog posts
  - E-commerce product listings
  - Help and documentation

  You should ask yourself: "Can I pre-render this page **ahead** of a user's request?" If the answer is yes, then you should choose Static Generation.

  On the other hand, Static Generation is **not** a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page shows frequently updated data, and the page content changes on every request.

  In that case, you can use **Server-Side Rendering**. It will be slower, but the pre-rendered page will always be up-to-date. Or you can skip pre-rendering and use client-side JavaScript to populate data.
  ```
* Parse the blog data by updating ``pages/index.js`` and installing [gray-matter](https://github.com/jonschlinkert/gray-matter) which lets us parse the metadata in each markdown file which will be configured in our custom library file ``lib/posts.js``
```
$ npm install gray-matter
```
```
import fs from 'fs'
import path from 'path'
import matter from 'gray-matter'

const postsDirectory = path.join(process.cwd(), 'posts')

export function getSortedPostsData() {
  // Get file names under /posts
  const fileNames = fs.readdirSync(postsDirectory)
  const allPostsData = fileNames.map(fileName => {
    // Remove ".md" from file name to get id
    const id = fileName.replace(/\.md$/, '')

    // Read markdown file as string
    const fullPath = path.join(postsDirectory, fileName)
    const fileContents = fs.readFileSync(fullPath, 'utf8')

    // Use gray-matter to parse the post metadata section
    const matterResult = matter(fileContents)

    // Combine the data with the id
    return {
      id,
      ...matterResult.data
    }
  })
  // Sort posts by date
  return allPostsData.sort((a, b) => {
    if (a.date < b.date) {
      return 1
    } else {
      return -1
    }
  })
}
```
* Import and call ``getSortedPostsData`` inside ``getStaticProps`` in ``pages/index.js`` above ``Home`` component
```
import { getSortedPostsData } from '../lib/posts'

export async function getStaticProps() {
  const allPostsData = getSortedPostsData()
  return {
    props: {
      allPostsData
    }
  }
}
```
* Pass the blog posts data in ``allPostData`` inside ``props`` object in ``getStaticProps`` to the ``Home`` component as a prop
```
export default function Home({ allPostsData }) {
  return (
    <Layout home>
      {/* Keep the existing code here */}

      {/* Add this <section> tag below the existing <section> tag */}
      <section className={`${utilStyles.headingMd} ${utilStyles.padding1px}`}>
        <h2 className={utilStyles.headingLg}>Blog</h2>
        <ul className={utilStyles.list}>
          {allPostsData.map(({ id, date, title }) => (
            <li className={utilStyles.listItem} key={id}>
              {title}
              <br />
              {id}
              <br />
              {date}
            </li>
          ))}
        </ul>
      </section>
    </Layout>
  )
}
```
* Function ``getStaticProps`` can also fetch external API or query database
  * fetch from an external API
  ```
  export async function getSortedPostsData() {
    // Instead of the file system,
    // fetch post data from an external API endpoint
    const res = await fetch('..')
    return res.json()
  }
  ```
  * query the database directly
  ```
  import someDatabaseSDK from 'someDatabaseSDK'

  const databaseClient = someDatabaseSDK.createClient(...)

  export async function getSortedPostsData() {
    // Instead of the file system,
    // fetch post data from a database
    return databaseClient.query('SELECT posts...')
  }
  ```
* In **development** (`npm run dev` or `yarn dev`) ``getStaticProps`` runs on every request but in production ``getStaticProps`` runs at build time and ``getStaticProps`` can only be exported from a page because React needs to have all the required data before the page is rendered
* To use Server-side Rendering we need to export ``getServerSideProps`` with its paramater (``context``) contains specific parameters and time to first byte ([TTFB](https://web.dev/time-to-first-byte/)) will be slower than ``getStaticProps``
```
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    }
  }
}
```
* We can use [Client-side Rendering](https://nextjs.org/docs/basic-features/data-fetching#fetching-data-on-the-client-side) which works well for user dashboard if we do not need to pre-render data and we can use a React hook for data fetching called [SWR](https://swr.vercel.app/) from the team behind Next.js
  * Statically generate (pre-render) parts of the page that do not require external data
  * Fetch the external data from the client using JavaScript and populate the remaining parts when the page loads

## Dynamic Routes

* We can statically generate pages with dynamic route for example for path **/posts/\<id\>** where **\<id\>** can be dynamic by creating a page at **pages/posts/[id].js** which contains
  * a React component to render this page
  * **getStaticPaths** which returns an array of possible values for **id**
  * **getStaticProps** which fetches necessary data for the post with **id**
* Implement getStaticPaths by creating file **pages/posts/[id].js** and also removing **pages/posts/first-post.js** because we'll no longer use this and we also need to update **lib/posts.js**
```
// lib/posts.js
export function getAllPostIds() {
  const fileNames = fs.readdirSync(postsDirectory)

  // Returns an array that looks like this:
  // [
  //   {
  //     params: {
  //       id: 'ssg-ssr'
  //     }
  //   },
  //   {
  //     params: {
  //       id: 'pre-rendering'
  //     }
  //   }
  // ]
  return fileNames.map(fileName => {
    return {
      params: {
        id: fileName.replace(/\.md$/, '')
      }
    }
  })
}
```
```
// pages/posts/[id].js
// we'll fill in `...` later
import Layout from '../../components/layout'
import { getAllPostIds } from '../../lib/posts'

export async function getStaticPaths() {
  const paths = getAllPostIds()
  return {
    paths,
    fallback: false
  }
}

export default function Post() {
  return <Layout>...</Layout>
}
```
* Implement ``getStaticProps`` to fetch necessary data to render the post with the given id by adding ``getPostData`` function to ``lib/posts.js`` and then import it to ``pages/posts/[id].js`` and update its content
```
// lib/posts.js
export function getPostData(id) {
  const fullPath = path.join(postsDirectory, `${id}.md`)
  const fileContents = fs.readFileSync(fullPath, 'utf8')

  // Use gray-matter to parse the post metadata section
  const matterResult = matter(fileContents)

  // Combine the data with the id
  return {
    id,
    ...matterResult.data
  }
}
```
```
// pages/posts/[id].js
import Layout from '../../components/layout'
import { getAllPostIds, getPostData } from '../../lib/posts'

export async function getStaticPaths() {
  const paths = getAllPostIds()
  return {
    paths,
    fallback: false
  }
}

export async function getStaticProps({ params }) {
  const postData = getPostData(params.id)
  return {
    props: {
      postData
    }
  }
}

export default function Post({ postData }) {
  return (
    <Layout>
      {postData.title}
      <br />
      {postData.id}
      <br />
      {postData.date}
    </Layout>
  )
}
```
* We'll use the ``remark`` library (`$ npm install remark remark-html`) to render markdown content and we need to update ``lib/posts.js`` and ``posts/pages/[id].js``
  * **lib/posts.js**
  ```
  //...
  import remark from 'remark'
  import html from 'remark-html'

  //...
  export async function getPostData(id) {
    const fullPath = path.join(postsDirectory, `${id}.md`)
    const fileContents = fs.readFileSync(fullPath, 'utf8')

    // Use gray-matter to parse the post metadata section
    const matterResult = matter(fileContents)

    // Use remark to convert markdown into HTML string
    const processedContent = await remark()
      .use(html)
      .process(matterResult.content)
    const contentHtml = processedContent.toString()

    // Combine the data with the id and contentHtml
    return {
      id,
      contentHtml,
      ...matterResult.data
    }
  }
  // ...
  ```
  * **pages/posts/[id].js**
  ```
  // ...
  export async function getStaticProps({ params }) {
    // Add the "await" keyword like this:
    const postData = await getPostData(params.id)
    // ...
  }
  // ...
  export default function Post({ postData }) {
    return (
      <Layout>
        {postData.title}
        <br />
        {postData.id}
        <br />
        {postData.date}
        <br />
        <div dangerouslySetInnerHTML={{ __html: postData.contentHtml }} />
      </Layout>
    )
  }
  // ...
  ```
* We can polish the post page ``pages/posts/[id].js`` by adding ``title`` to the post page and formatting the date by using ``date-fns`` (`$ npm install date-fns`) and adding the CSS
  * We need to create a new ``Date`` component by creating a file called ``components/date.js``
  ```
  import { parseISO, format } from 'date-fns'

  export default function Date({ dateString }) {
    const date = parseISO(dateString)
    return <time dateTime={dateString}>{format(date, 'LLLL d, yyyy')}</time>
  }
  ```
```
import Layout from '../../components/layout'
import { getAllPostIds, getPostData } from '../../lib/posts'
import Head from 'next/head'
import Date from '../../components/date'
import utilStyles from '../../styles/utils.module.css'

export async function getStaticPaths() {
  const paths = getAllPostIds()
  return {
    paths,
    fallback: false
  }
}

export async function getStaticProps({ params }) {
  // Add the "await" keyword like this:
  const postData = await getPostData(params.id)
  return {
    props: {
      postData
    }
  }
}

export default function Post({ postData }) {
  return (
    <Layout>
      <Head>
        <title>{postData.title}</title>
      </Head>
      <article>
        <h1 className={utilStyles.headingXl}>{postData.title}</h1>
        <div className={utilStyles.lightText}>
          <Date dateString={postData.date} />
        </div>
        <div dangerouslySetInnerHTML={{ __html: postData.contentHtml }} />
      </article>
    </Layout>
  )
}
```
* We can polish the index page ``pages/index.js`` by adding ``Link`` and ``Date`` components
```
// ...
import Link from 'next/link'
import Date from '../components/date'
// ...
<li className={utilStyles.listItem} key={id}>
  <Link href={`/posts/${id}`}>
    <a>{title}</a>
  </Link>
  <br />
  <small className={utilStyles.lightText}>
    <Date dateString={date} />
  </small>
</li>
// ...
```
* Dynamic routes details
  * Function ``getAllPostIds`` can fetch from external API endpoint
  ```
  export async function getAllPostIds() {
    // Instead of the file system,
    // fetch post data from an external API endpoint
    const res = await fetch('..')
    const posts = await res.json()
    return posts.map(post => {
      return {
        params: {
          id: post.id
        }
      }
    })
  }
  ```
  * Function ``getStaticPaths`` runs on every request in development but runs at build time in production
  * Parameter [``fallback``](https://nextjs.org/docs/basic-features/data-fetching#the-fallback-key-required) changes the behavior of ``getStaticPaths`` and ``getStaticProps``
  * Dynamic routes can be extended to [catch all](https://nextjs.org/docs/routing/dynamic-routes#catch-all-routes) paths by adding three dots (``...``) inside the brackets ``pages/posts/[...id].js`` and ``getStaticPaths`` must return an array as the value of the ``id`` key
  ```
  return [
    {
      params: {
        // Statically Generates /posts/a/b/c
        id: ['a', 'b', 'c']
      }
    }
    //...
  ]
  ```
  * and ``params.id`` will be an array in ``getStaticProps``
  ```
  export async function getStaticProps({ params }) {
    // params.id will be like ['a', 'b', 'c']
  }
  ```
  * Import ``useRouter`` hook from ``next/router`` if you want to access the Next.js router
  * Create custom 404 page by creating file ``pages/404.js`` which statically generated at build time and see also [Error Pages](https://nextjs.org/docs/advanced-features/custom-error-page)
  ```
  // pages/404.js
  export default function Custom404() {
    return <h1>404 - Page Not Found</h1>
  }
  ```

## API Routes

* API Routes let us create an API endpoint inside a Next.js app by creating a function inside ``pages/api`` directory with the following format
```
  // req = HTTP incoming message, res = HTTP server response
export default function handler(req, res) {
  // ...
}
```
* A simple endpoint ``api/hello`` from file ``pages/api/hello.js``
```
export default function handler(req, res) {
  res.status(200).json({ text: 'Hello' })
}
```
* You should not fetch an API Route from ``getStaticProps`` or ``getStaticPaths`` but write your server-side code directly in ``getStaticProps`` or ``getStaticPaths`` (or call a helper function)
* A good use case for API Routes is handling form input i.e. a form on your page and have it send a POST request to your API Route
```
export default function handler(req, res) {
  const email = req.body.email
  // Then save email to your database, etc...
}
```
* [Preview Mode](https://nextjs.org/docs/advanced-features/preview-mode)
* [Dynamic API Routes](https://nextjs.org/docs/api-routes/dynamic-api-routes)

## Deploying Your Next.js App

* We will need a [Github account](https://github.com/)
* Push our app to a repository in the Github
```
git remote add origin https://github.com/<username>/nextjs-blog.git
git push -u origin main
```
* Create a [Vercel](https://vercel.com/) account and import your ``nextjs-blog`` repository on Vercel
* Vercel is made by the creator of Next.js and supports the following features
  * Pages that use Static Generation and assets (JS, CSS, images, fonts, etc) is automatically served from [Vercel Edge Network](https://vercel.com/docs/edge-network/overview)
  * Pages that use Server-side rendering and API routes automatically become isolated [Serverless Function](https://vercel.com/docs/serverless-functions/introduction)
  * You can assign a [custom domain](https://vercel.com/docs/custom-domains) to your Next.js app
  * You can also [set environment variables](https://vercel.com/docs/build-step#environment-variables) on Vercel and [use them](https://nextjs.org/docs/basic-features/environment-variables#loading-environment-variables)
  * HTTPS is enabled by default (including custom domains) and doesn't require extra configuration and support auto-renewal
* We can preview our deployment for every push using Vercel, every time we create a pull request for a branch the ``vercel`` bot will comment on the PR page with a **Preview** URL and if we satisfied with the preview we can merge it to ``main`` branch
* The Next.js deployment workflow using Vercel is called **DPS**: **D**evelop, **P**review, and **S**hip
* Next.js can be deployed to any hosting provider that supports Node.js
  * Check the ``package.json`` for ``build`` and ``start`` scripts
  ```
  {
    "scripts": {
      "dev": "next",
      "build": "next build",
      "start": "next start"
    }
  }
  ```
  * Execute the ``build`` and ``start`` script to host Next.js on the other provider
  ```
  $ npm run build
  $ npm run start
  ```
  * Customize ``package.json`` to use custom port
  ```
  {
    "scripts": {
      "dev": "next",
      "build": "next build",
      "start": "next start -p $PORT"
    }
  }
  ```