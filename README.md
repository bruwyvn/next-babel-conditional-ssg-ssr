# Plugin to Enable Conditional SSG / SSR in Next.js

This plugin allows you to have `getStaticProps`, `getStaticPaths` and `getServerSideProps` all on the same page and have them conditionally enabled depending on the build mode. Avoiding the dreaded `SERVER_PROPS_SSG_CONFLICT` error message of **You can not use getStaticProps or getStaticPaths with getServerSideProps. To use SSG, please remove getServerSideProps**

At the moment the plugin only works when customizing the project's Babel configuration, disabling SWC.

This plugin should be considered experimental and has largely been based on internal Next.js plugins, as noted in the comments.

So, I wanted a way to use SSG for a hybrid app using [capacitor](https://capacitorjs.com/), and discovered that setting this up on Next.js is not as easy as on Nuxt.js, and going down the rabbit hole I have found [this example](https://github.com/mlynch/nextjs-tailwind-ionic-capacitor-starter) and @erzr's code, but it did not work with Next.js 12, and it was stale with an open PR with a fix for it.

This fork includes a [PR](https://github.com/erzr/next-babel-conditional-ssg-ssr/pull/4) fix mentioned above (thanks @Anthonyzou), and package changes, so I can publish it on NPM, and this README :)

## Usage

First, start by add this project as a developer dependency using NPM, pnpm or yarn.

```bash
# NPM
npm install -D nextjs-conditional-ssg-ssr
```

```bash
# pnpm
pnpm add -D nextjs-conditional-ssg-ssr
```

```bash
# yarn
yarn add -D nextjs-conditional-ssg-ssr
```

After that, one way to integrate this is to create a `babel.config.js` in the root of your Next.js project. Here's what that would look like:

```javascript
// babel.config.js

const nextModeBabelPlugin = require("nextjs-conditional-ssg-ssr");

const presets = ["next/babel"];
const plugins = [nextModeBabelPlugin(`process.env.BUILD_MODE` ?? "ssr")];

module.exports = { presets, plugins };
```

This example is configured for `ssr` as the default build mode, but as noted `ssg` is also a valid value. Then, add a `BUILD_MODE` variable to the `build` and `export` scripts:

```json
// package.json

{
  "scripts": {
    // ...
    "build": "BUILD_MODE=ssr next build",
    "export": "BUILD_MODE=ssg next build && next export"
  }
}
```
