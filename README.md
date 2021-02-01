# igloo

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm build
$ npm start

# generate static project
$ yarn generate
```

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).

# Docker

Til að keyra docker þarf að gera build skipun fyrst.

docker build -t igloo .

Svo þarf að gera run skipun:

docker run -p 5000:5000 igloo


# heroku

Verkefnið keyrir á heroku: https://nuxt-js-my-igloo.herokuapp.com/

