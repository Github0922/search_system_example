A simple javascript library for working with [ElasticSearch][].

It also provides a backend interface to ElasticSearch suitable for use with the
[Recline][] suite of data libraries.

[ElasticSearch]: http://www.elastic.co/
[Recline]: http://okfnlabs.org/recline/

## Usage

The library requires:

* underscore
* jQuery (for Ajax requests)

Load the library in your web page, e.g. do:

    <script type="text/javascript" src="http://okfnlabs.org/elasticsearch.js/elasticsearch.js"></script>

(You can replace the src url with the url to your copy of elasticsearch.js).

## Example

Here's an example of using the library to create, get and query some data.

    // Your ElasticSearch instance is running at http://localhost:9200/
    // We are using index 'twitter' and type (table) 'tweet'
    var endpoint = 'http://localhost:9200/twitter/tweet';

    // Table = an ElasticSearch Type (aka Table)
    // http://www.elasticsearch.org/guide/reference/glossary/#type
    var table = ES.Table(endpoint);

    // Create some data
    table.upsert({
      id: '123',
      title: 'My new tweet'
    }).done(function() {
      // now get it
      table.get('123').done(function(doc) {
        console.log(doc);
      });
    });

    // Query for data
    // Queries follow Recline Query spec -
    // http://okfnlabs.org/recline/docs/models.html#query-structure
    // (very similar to ES)
    table.query({
      q: 'hello'
      filters: [
        { term: { 'owner': 'jones' } }
      ]
    }).done(function(out) {
      console.log(out);
    });

    // get the mapping for this "table"
    // http://www.elasticsearch.org/guide/reference/glossary/#mapping
    table.mapping().done(function(theMapping) {
      console.log(theMapping)
    });

