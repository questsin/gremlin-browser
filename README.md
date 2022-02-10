# gremlin-browser
JavaScript Gremlin Language Variant for the Browser based on version gremlin-js v3.5.2 and isomorphic-ws v4.0.1

* [gremlin-javascript for node](https://www.npmjs.com/package/gremlin)
* [isomorphic-ws](https://www.npmjs.com/package/isomorphic-ws)

Tested using 
* [Azure Cosmos DB Gremlin API](https://docs.microsoft.com/en-us/azure/cosmos-db/graph/graph-introduction)

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
The Gremlin language allows users to write highly expressive graph traversals and has a broad list of functions that cover a wide body of features. The Reference Documentation describes these functions and other aspects of the TinkerPop ecosystem including some specifics on Gremlin in Javascript itself. Most of the examples found in the documentation use Groovy language syntax in the Gremlin Console. For the most part, these examples should generally translate to Javascript with little modification. Given the strong correspondence between canonical Gremlin in Java and its variants like Javascript, there is a limited amount of Javascript-specific documentation and examples. This strong correspondence among variants ensures that the general Gremlin reference documentation is applicable to all variants and that users moving between development languages can easily adopt the Gremlin variant for that language.

## Create Vertex
```javascript
/* if we want to assign our own ID and properties to this vertex */
const { t: { id } } = gremlin.process;
const { cardinality: { single } } = gremlin.process;

/**
 * Create a new vertex with Id, Label and properties
 * @param {String,Number} vertexId Vertex Id (assuming the graph database allows id assignment)
 * @param {String} vlabel Vertex Label
 */
const createVertex = async (vertexId, vlabel) => {
  const vertex = await g.addV(vlabel)
    .property(id, vertexId)
    .property(single, 'name', 'Apache')
    .property('lastname', 'Tinkerpop') // default database cardinality
    .next();

  return vertex.value;
};
```
## Find Vertices
```javascript
/**
 * List all vertexes in db
 * @param {Number} limit
 */
const listAll = async (limit = 500) => {
  return g.V().limit(limit).elementMap().toList();
};
/**
 * Find unique vertex with id
 * @param {Object} vertexId Vertex Id
 */
const findVertex = async (vertexId) => {
  const vertex = await g.V(vertexId).elementMap().next();
  return vertex.value;
};
/**
 * Find vertices by label and 'name' property
 * @param {String} vlabel Vertex label
 * @param {String} name value of 'name' property
 */
const listByLabelAndName = async (vlabel, name) => {
  return g.V().has(vlabel, 'name', name).elementMap().toList();
};
## Update Vertex
const { cardinality: { single } } = gremlin.process;

/**
 * Update Vertex Properties
 * @param {String,Number} vertexId Vertex Id
 * @param {String} name Vertex Name Property
 */
const updateVertex = async (vertexId, label, name) => {
  const vertex = await g.V(vertexId).property(single, 'name', name).next();
  return vertex.value;
};
```
NOTE that versions suffixed with "-rc" are considered release candidates (i.e. pre-alpha, alpha, beta, etc.) and thus for early testing purposes only.