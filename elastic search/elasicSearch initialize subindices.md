
You can use the Elasticsearch module for Node.js to perform sub-indexing operations with code. Here is an example:

```javascript
const { Client } = require('@elastic/elasticsearch');

// Create a client instance
const client = new Client({ node: 'http://localhost:9200' });

// Define the index settings and mappings
const indexSettings = {
  settings: {
    number_of_shards: 2,
    number_of_replicas: 1
  },
  mappings: {
    properties: {
      name: { type: 'text' },
      age: { type: 'integer' }
    }
  }
};

// Create the index and set the aliases
async function createSubIndex(indexName, aliasName) {
  // Create the index with the settings and mappings
  await client.indices.create({
    index: indexName,
    body: indexSettings
  });

  // Add the alias to point to the new index
  await client.indices.updateAliases({
    body: {
      actions: [
        { add: { index: indexName, alias: aliasName } }
      ]
    }
  });
}

// Perform a search on the sub-index using the alias
async function searchSubIndex(aliasName, query) {
  const { body } = await client.search({
    index: aliasName,
    body: { query }
  });
  return body.hits.hits;
}
```

In this example, the `createSubIndex` function creates a new index with the specified name and applies the defined settings and mappings. It then adds an alias to point to the new index.

The `searchSubIndex` function performs a search on the sub-index using the specified alias name and query.

To use these functions in your code, you can call them as follows:

```
await createSubIndex('my_index', 'my_alias');
const results = await searchSubIndex('my_alias', { match: { name: 'John' } });
console.log(results);
```

This will create a new index called `my_index` with the settings and mappings defined in `indexSettings`, and an alias called `my_alias` that points to the new index. It then performs a search on the sub-index using the `my_alias` alias and a query for documents where the `name` field matches "John". The search results are logged to the console.
