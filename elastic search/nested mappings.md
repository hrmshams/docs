To define complex nested mappings in Elasticsearch using its Node.js module, you can use the `mappings` property of the Elasticsearch client's index creation options. Here's an example:

```javascript
const { Client } = require('@elastic/elasticsearch');

const client = new Client({ node: 'http://localhost:9200' });

client.indices.create({
  index: 'my_index',
  body: {
    mappings: {
      properties: {
        field1: { type: 'keyword' },
        field2: {
          type: 'nested',
          properties: {
            subfield1: { type: 'text' },
            subfield2: { type: 'integer' }
          }
        }
      }
    }
  }
}, (err, resp, status) => {
  if (err) {
    console.error(err);
  } else {
    console.log("Index created with mapping:", resp);
  }
});
```
