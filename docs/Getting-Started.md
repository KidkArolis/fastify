<h1 align="center">Fastify</h1>

## Getting Started

### Install
```
npm i fastify --save
```
### Your first server
Let's write a *Hello world* server!
```js
// Require the framework and instantiate it
const fastify = require('fastify')()

// Declare a route
fastify.get('/', function (request, reply) {
  reply.send({ hello: 'world' })
})

// Run the server!
fastify.listen(3000, function (err) {
  if (err) throw err
  console.log(`server listening on ${fastify.server.address().port}`)
})
```
- `fastify.server`: The Node core server object
- `fastify.ready(callback)`: Function called when all the plugins has been loaded.
- `fastify.listen(callback)`: Starts the server on the given port after all the plugins are loaded, internally waits for the `.ready()` event. The callback is the same as the Node core.
- `fastify.route(options)`: check [here](https://github.com/fastify/fastify/blob/master/docs/Routes.md).

<a name="schema"></a>
### Schema serialization
How get better performances?  
Fastify is designed to be very performant, if you want to make it even faster, you can use JSON schema to speed up the serialization of your output.
```js
const fastify = require('fastify')()

const schema = {
  out: {
    type: 'object',
    properties: {
      hello: { type: 'string' }
    }
  }
}

// Declare a route with an output schema
fastify.get('/', schema, function (request, reply) {
  reply.send({ hello: 'world' })
})

fastify.listen(3000, function (err) {
  if (err) throw err
  console.log(`server listening on ${fastify.server.address().port}`)
})
```
*If you want to know more about the serialization and validation, check [here](https://github.com/fastify/fastify/blob/master/docs/Validation-And-Serialize.md)!*

<a name="register"></a>
### Register
As you can see use Fastify is very easy and handy.
Obviously register all the routes in the same file is not a good idea, Fastify offers an utility to solve this problem, `register`.

```js
/* server.js */

const fastify = require('fastify')()

fastify.register(require('./route'), function (err) {
  if (err) throw err
})

const opts = {
  hello: 'world',
  something: true
}
fastify.register([
  require('./another-route'),
  require('./yet-another-route')
], opts, function (err) {
  if (err) throw err
})

fastify.listen(8000, function (err) {
  if (err) throw err
  console.log(`server listening on ${fastify.server.address().port}`)
})
```
```js
/* route.js */

module.exports = function (fastify, options, next) {
  fastify.get('/', schema, function (req, reply) {
    reply.send({ hello: 'world' })
  })
  next()
}
```

<a name="next"></a>
### Next
Do you want to know more?
Check the documentation folder [here](https://github.com/fastify/fastify/blob/master/docs/)!
