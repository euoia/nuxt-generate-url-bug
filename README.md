# nuxt-generate-url-bug

This repository demonstrates a bug with `nuxt generate` where relative URLs in
CSS files are converted to absolute URLs even when `build.publicPath` is
relative in `nuxt.config`.

The use case here is when you are trying to produce a completely static site
than can be deployed to any subdirectory. That is, you don't know ahead of time
whether it will be deployed to https://example.com/ or
https://https://example.com/myamazingwebsite etc.

In these cases, all assets must be referenced relatively.

## Changes from create-nuxt-app

Check the commit log for the complete list of changes, but in general, after running `create-nuxt-app`:

1. In nuxt.config.js `build.publicPath` was set to `./_nuxt`:

```
  // Build Configuration: https://go.nuxtjs.dev/config-build
  build: {
    publicPath: "./_nuxt"
  }
```

2. In nuxt.config a css file was added: `css: ["~/assets/main.css"],`

3. An assets directory was created and an `assets/main.css` file was created.

4. `assets/main.css` references a font using a relative URL reference.

## How to Test

Test whether asset loading is working inside a subdirectory by serving the root
of this repository and attempting to load the `/dist` directory.

1. Clone this repo
2. Run `npm install`
3. Run `nuxt generate`

The `dist/` directory will contain the static site.

Serve the root directory of this project. The static static site will be
served from `/dist`. For example:

```
python3 -m http.server
```

You should see something like the following:

```
Serving HTTP on :: port 8000 (http://[::]:8000/) ...
```

6. Go to http://localhost:8000/dist in your browser.
7. Observe that the fonts, which are included from main.css fail to load.

You should see an error in the browser console:

```
GET http://localhost:8000/_nuxt/fonts/Open_Sans-700-latin20.55397be.woff2 net::ERR_ABORTED 404 (File not found)
```
