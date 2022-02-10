# gremlin-browser
JavaScript Gremlin Language Variant for the Browser

## JavaScript Gremlin Language Variant for the Browser
Apache TinkerPop™ is a graph computing framework for both graph databases (OLTP) and graph analytic systems (OLAP). Gremlin is the graph traversal language of TinkerPop. It can be described as a functional, data-flow language that enables users to succinctly express complex traversals on (or queries of) their application's property graph.

Gremlin-browser is a simply browserify version after patching ws to work with ws-isomorphic

Gremlin-Javascript is designed to connect to a "server" that is hosting a TinkerPop-enabled graph system. That "server" could be Gremlin Server or a remote Gremlin provider that exposes protocols by which Gremlin-Javascript can connect.

A typical connection to a server running on "localhost" that supports the Gremlin Server protocol using websockets looks like this:

```javascript
const gremlin = Window.gremlin;
const traversal = gremlin.process.AnonymousTraversalSource.traversal;
const DriverRemoteConnection = gremlin.driver.DriverRemoteConnection;

const g = traversal().withRemote(new DriverRemoteConnection('ws://localhost:8182/gremlin'));
```
Once "g" has been created using a connection, it is then possible to start writing Gremlin traversals to query the remote graph:

```javascript
g.V().hasLabel('person').values('name').toList()
  .then(names => console.log(names));

const names = await g.V().hasLabel('person').values('name').toList();
console.log(names);
```

# Sample Traversals