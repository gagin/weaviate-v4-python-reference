Condensed Weaviate v4 Python Client Reference with Examples
===========================================================

This is a concise guide to the Weaviate v4 Python client, including key
functionality and examples. Links point to the full reference in
`reference.md <reference.md>`__.

.. container:: toc

   .. rubric:: Table of Contents
      :name: table-of-contents

   - `Setup and Connection <#setup-and-connection>`__

     - `weaviate Module <#weaviate-module>`__

   - `Client Object <#client-object>`__
   - `Collections Management <#collections-management>`__
   - `Collection Operations <#collection-operations>`__

     - `config - CollectionConfig <#config-collectionconfig>`__
     - `data - CollectionData <#data-collectiondata>`__
     - `query - CollectionQuery <#query-collectionquery>`__

   - `classes Module Highlights <#classes-module>`__

     - `config Submodule <#config-submodule>`__
     - `query Submodule <#query-submodule>`__
     - `init Submodule <#init-submodule>`__
     - `data Submodule <#data-submodule>`__

   - `Notes <#notes>`__

Setup and Connection
--------------------

`weaviate <reference.md#weaviate-module>`__ Module
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- `connect_to_local <reference.md#connect_to_local>`__: Connect to a
  local Weaviate instance.

  ::

     import weaviate
     client = weaviate.connect_to_local()
     print(client.is_ready())  # True if connected
     client.close()

- `connect_to_weaviate_cloud <reference.md#connect_to_weaviate_cloud>`__:
  Connect to Weaviate Cloud.

  ::

     from weaviate.classes.init import Auth
     client = weaviate.connect_to_weaviate_cloud(
         cluster_url="https://my-cluster.weaviate.cloud",
         auth_credentials=Auth.api_key("my-api-key")
     )
     client.close()

- `connect_to_embedded <reference.md#connect_to_embedded>`__: Run
  Weaviate embedded.

  ::

     client = weaviate.connect_to_embedded()
     client.close()

- `connect_to_custom <reference.md#connect_to_custom>`__: Custom
  connection.

  ::

     client = weaviate.connect_to_custom(
         http_host="localhost",
         http_port=8080,
         http_secure=False,
         grpc_host="localhost",
         grpc_port=50051,
         grpc_secure=False
     )
     client.close()

`Client <reference.md#client-object>`__ Object
----------------------------------------------

- `close <reference.md#close>`__: Close the client connection.

  ::

     client.close()

- `collections <reference.md#collections>`__: Access collection
  management.

  ::

     collections = client.collections

`Collections <reference.md#collections-object>`__ Management
------------------------------------------------------------

- `create <reference.md#create>`__: Create a new collection.

  ::

     from weaviate.classes.config import Property, DataType, Configure
     collections = client.collections
     collections.create(
         name="Article",
         properties=[
             Property(name="title", data_type=DataType.TEXT),
             Property(name="content", data_type=DataType.TEXT)
         ],
         vectorizer_config=Configure.Vectorizer.text2vec_openai()
     )

- `get <reference.md#get>`__: Retrieve a collection.

  ::

     article_collection = client.collections.get(name="Article")

- `delete <reference.md#delete>`__: Delete a collection.

  ::

     client.collections.delete(name="Article")

- `list_all <reference.md#list_all>`__: List all collections.

  ::

     all_collections = client.collections.list_all(simple=True)
     print(all_collections)

`Collection <reference.md#collection-object>`__ Operations
----------------------------------------------------------

.. _config-collectionconfig:

`config <reference.md#config>`__ - `CollectionConfig <reference.md#collectionconfig-object>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- `get <reference.md#collectionconfig-get>`__: Get collection config.

  ::

     config = article_collection.config.get()
     print(config)

- `add_property <reference.md#add_property>`__: Add a property.

  ::

     article_collection.config.add_property(
         property=Property(name="author", data_type=DataType.TEXT)
     )

.. _data-collectiondata:

`data <reference.md#data>`__ - `CollectionData <reference.md#collectiondata-object>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- `insert <reference.md#insert>`__: Insert a data object.

  ::

     uuid = article_collection.data.insert(
         properties={"title": "Hello World", "content": "Weaviate v4 rocks!"}
     )
     print(uuid)

- `exists <reference.md#exists>`__: Check if an object exists.

  ::

     exists = article_collection.data.exists(uuid=uuid)
     print(exists)  # True

.. _query-collectionquery:

`query <reference.md#query>`__ - `CollectionQuery <reference.md#collectionquery-object>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- `bm25 <reference.md#bm25>`__: Keyword search.

  ::

     response = article_collection.query.bm25(
         query="Hello",
         limit=2
     )
     for obj in response.objects:
         print(obj.properties["title"])

- `near_text <reference.md#near_text>`__: Semantic search.

  ::

     from weaviate.classes.query import MetadataQuery
     response = article_collection.query.near_text(
         query="greeting",
         limit=1,
         return_metadata=MetadataQuery(distance=True)
     )
     print(response.objects[0].properties, response.objects[0].metadata.distance)

- `fetch_object_by_id <reference.md#fetch_object_by_id>`__: Fetch by
  UUID.

  ::

     obj = article_collection.query.fetch_object_by_id(
         uuid=uuid,
         include_vector=True
     )
     print(obj.properties, obj.vector)

.. _classes-module:

`classes <reference.md#classes-module>`__ Module Highlights
-----------------------------------------------------------

`config <reference.md#config-submodule>`__ Submodule
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- `Configure <reference.md#configure>`__: Configuration helpers.

  ::

     vectorizer = Configure.Vectorizer.text2vec_openai()
     index = Configure.VectorIndex.hnsw()

- `Property <reference.md#property>`__: Define properties.

  ::

     prop = Property(name="title", data_type=DataType.TEXT)

`query <reference.md#query-submodule>`__ Submodule
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- `Filter <reference.md#filter>`__: Query filters.

  ::

     from weaviate.classes.query import Filter
     response = article_collection.query.bm25(
         query="Hello",
         filters=Filter.by_property("title").equal("Hello World")
     )

`init <reference.md#init-submodule>`__ Submodule
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- `Auth <reference.md#auth>`__: Authentication.

  ::

     auth = Auth.api_key("my-api-key")

`data <reference.md#data-submodule>`__ Submodule
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- `DataObject <reference.md#dataobject>`__: Represents retrieved
  objects.

  ::

     obj = article_collection.query.fetch_object_by_id(uuid)
     print(obj.properties["title"])

- `QueryResponse <reference.md#queryresponse>`__: Query results.

  ::

     response = article_collection.query.near_text(query="greeting")
     for obj in response.objects:
         print(obj.properties)

Notes
-----

- This condensed reference assumes a basic Weaviate setup and focuses on
  common use cases.
- Examples use synchronous calls; for async, use ``WeaviateAsyncClient``
  or ``weaviate.use_async_with_*`` (see docs).
- Links assume the structure of `reference.md <reference.md>`__. Adjust
  if the file structure differs.
- Missing methods (e.g., batch operations, ``near_vector``) can be added
  if needed—contact for updates!
