<p algin="center"><img src="https://raw.githubusercontent.com/CLARIAH/grlc/master/static/img/grlc_logo_01.png" width="250px"></p>

grlc, the <b>g</b>it <b>r</b>epository <b>l</b>inked data API <b>c</b>onstructor, automatically builds Web APIs using SPARQL queries stored in git repositories.

**Author:**	Albert Meroño  
**Copyright:**	Albert Meroño, VU University Amsterdam  
**License:**	MIT License (see [license.txt](license.txt))

### Install and run

<pre>
git clone --recursive https://github.com/CLARIAH/grlc
cd grlc
virtualenv .
source bin/activate
pip install -r requirements.txt
python grlc.py
</pre>

Direct your browser to [http://localhost:8088](http://localhost:8088).

Alternatively, you can use the provided Gunicorn configuration to run it as a daemon on your server.

### Usage

grlc assumes a GitHub repository (support for general git repos is on the way) where you store your SPARQL queries as .rq files (like in [this one](https://github.com/CEDAR-project/Queries)). grlc will create an API operation per such a SPARQL query/.rq file.

If you're seeing this, your grlc instance is up and running, and ready to build APIs. Assuming you got it running at <code>http://localhost:8088/</code> and your queries are at <code>https://github.com/CEDAR-project/Queries</code>, just point your browser to the following locations:

- To request the swagger spec of your API, <code>http://localhost:8088/username/repo/spec</code>, e.g. [http://localhost:8088/CEDAR-project/Queries/spec](http://localhost:8088/CEDAR-project/Queries/spec) or [http://localhost:8088/CLARIAH/wp4-queries/spec](http://localhost:8088/CLARIAH/wp4-queries/spec)
- To request the api-docs of your API swagger-ui style, <code>http://localhost:8088/username/repo/api-docs</code>, e.g. [http://localhost:8088/CEDAR-project/Queries/api-docs](http://localhost:8088/CEDAR-project/Queries/api-docs) or [http://localhost:8088/CLARIAH/wp4-queries/api-docs](http://localhost:8088/CLARIAH/wp4-queries/api-docs)

By default grlc will direct your queries to the DBPedia SPARQL endpoint. To change this either:

* Add a `endpoint:` decorator in the first comment block of the query text (preferred, see below)
* Add the URL of the endpoint on a single line in an `endpoint.txt` file within the GitHub repository that contains the queries.
* Or you can directly modify the grlc source code (but it's nicer if the queries are self-contained)

That's it!

### Decorator syntax
A couple of SPARQL comment embedded decorators are available to make your swagger-ui look nicer (note all comments start with <code>#+ </code>):

- To specify a query-specific endpoint, <code>#+ endpoint: http://example.com/sparql</code>.
- To create a summary of your query/operation, <code>#+ summary: This is the summary of my query/operation</code>
- To assign tags to your query/operation,
    <pre>&#35;+ tags:
  &#35;+   - firstTag
  &#35;+   - secondTag</pre>

See examples at [https://github.com/CEDAR-project/Queries](https://github.com/CEDAR-project/Queries).

### Request parameter mappings into SPARQL

grlc is compliant with [BASIL's convention](https://github.com/the-open-university/basil/wiki/SPARQL-variable-name-convention-for-WEB-API-parameters-mapping) on how to map GET/POST request parameters into SPARQL.
