# Weaviate v4 Python Client Reference

## Table of Contents

- [`weaviate` Module](#weaviate-module)
  - [Functions](#functions)
    - [`connect_to_local`](#connect_to_local)
    - [`connect_to_weaviate_cloud`](#connect_to_weaviate_cloud)
    - [`connect_to_embedded`](#connect_to_embedded)
    - [`connect_to_custom`](#connect_to_custom)
- [`Client` Object](#client-object)
  - [Methods](#client-methods)
    - [`close`](#close)
    - [`is_ready`](#is_ready)
    - [`get_meta`](#get_meta)
  - [Attributes](#client-attributes)
    - [`collections`](#collections)
- [`Collections` Object](#collections-object)
  - [Methods](#collections-methods)
    - [`create`](#create)
    - [`create_from_dict`](#create_from_dict)
    - [`get`](#get)
    - [`exists`](#exists)
    - [`list_all`](#list_all)
    - [`delete`](#delete)
    - [`delete_all`](#delete_all)
- [`Collection` Object](#collection-object)
  - [Attributes](#collection-attributes)
    - [`config`](#config)
    - [`data`](#data)
    - [`query`](#query)
- [`CollectionConfig` Object](#collectionconfig-object)
  - [Methods](#collectionconfig-methods)
    - [`get`](#collectionconfig-get)
    - [`update`](#update)
    - [`add_property`](#add_property)
    - [`get_shards`](#get_shards)
    - [`update_shards`](#update_shards)
- [`CollectionData` Object](#collectiondata-object)
  - [Methods](#collectiondata-methods)
    - [`insert`](#insert)
    - [`insert_many`](#insert_many)
    - [`update`](#update)
    - [`delete`](#delete)
    - [`replace`](#replace)
    - [`exists`](#exists)
- [`CollectionQuery` Object](#collectionquery-object)
  - [Methods](#collectionquery-methods)
    - [`bm25`](#bm25)
    - [`near_text`](#near_text)
    - [`near_vector`](#near_vector)
    - [`near_object`](#near_object)
    - [`hybrid`](#hybrid)
    - [`aggregate`](#aggregate)
    - [`fetch_object_by_id`](#fetch_object_by_id)
- [`classes` Module](#classes-module)
  - [`config` Submodule](#config-submodule)
    - [`Configure`](#configure)
    - [`Property`](#property)
    - [`DataType`](#datatype)
    - [`Tokenization`](#tokenization)
    - [`VectorDistances`](#vectordistances)
  - [`query` Submodule](#query-submodule)
    - [`Move`](#move)
    - [`MetadataQuery`](#metadataquery)
    - [`Filter`](#filter)
  - [`init` Submodule](#init-submodule)
    - [`Auth`](#auth)
    - [`Timeout`](#timeout)
  - [`data` Submodule](#data-submodule)
    - [`DataObject`](#dataobject)
    - [`QueryResponse`](#queryresponse)
    - [`Reference`](#reference)

---

## [`weaviate` Module](#weaviate-module)

### Functions

#### [`connect_to_local`](#connect_to_local)
* Description: Connects to a local Weaviate instance on default host (localhost:8080) and gRPC port (50051). Supports `with` statement for auto-closing.
* Returns: [`Client`](#client-object) object
* Usage: `with weaviate.connect_to_local() as client: ...`

#### [`connect_to_weaviate_cloud`](#connect_to_weaviate_cloud)
* Description: Connects to a Weaviate Cloud Services instance.
* Parameters:
  * `cluster_url` (str): URL of the Weaviate Cloud cluster.
  * `auth_credentials` ([`Auth`](#auth)): Authentication credentials (e.g., API key).
  * `headers` (dict, optional): Custom HTTP headers.
  * `timeout` ([`Timeout`](#timeout), optional): Request timeout settings.
* Returns: [`Client`](#client-object) object

#### [`connect_to_embedded`](#connect_to_embedded)
* Description: Initializes and connects to an embedded Weaviate instance within the Python process.
* Parameters:
  * `additional_env_vars` (dict, optional): Environment variables for embedded instance.
  * `timeout` ([`Timeout`](#timeout), optional): Timeout settings.
* Returns: [`Client`](#client-object) object

#### [`connect_to_custom`](#connect_to_custom)
* Description: Connects to a Weaviate instance with custom HTTP and gRPC settings.
* Parameters:
  * `http_host` (str): HTTP host address.
  * `http_port` (int): HTTP port number.
  * `http_secure` (bool): Use HTTPS if True.
  * `grpc_host` (str): gRPC host address.
  * `grpc_port` (int): gRPC port number.
  * `grpc_secure` (bool): Use gRPC secure connection if True.
  * `headers` (dict, optional): Custom HTTP headers.
  * `timeout` ([`Timeout`](#timeout), optional): Timeout settings.
* Returns: [`Client`](#client-object) object

---

## [`Client` Object](#client-object)

### Methods

#### [`close`](#close)
* Description: Closes the connection to the Weaviate instance, freeing resources.

#### [`is_ready`](#is_ready)
* Description: Checks if the Weaviate instance is ready to accept requests.
* Returns: bool (True if ready)

#### [`get_meta`](#get_meta)
* Description: Retrieves metadata about the Weaviate instance (e.g., version, modules).
* Returns: dict (Metadata)

### Attributes

#### [`collections`](#collections)
* Type: [`Collections`](#collections-object) object
* Description: Interface for managing collections.

---

## [`Collections` Object](#collections-object)

### Methods

#### [`create`](#create)
* Description: Creates a new collection with specified configuration.
* Parameters:
  * `name` (str): Unique collection name.
  * `properties` (List[[`Property`](#property)], optional): List of collection properties.
  * `vectorizer_config` ([`Configure.Vectorizer`](#configure), optional): Vectorizer settings.
  * `vector_index_config` ([`Configure.VectorIndex`](#configure), optional): Vector index settings.
  * `inverted_index_config` ([`Configure.inverted_index`](#configure), optional): Inverted index settings.
  * `replication_config` ([`Configure.replication`](#configure), optional): Replication settings.
  * `sharding_config` ([`Configure.sharding`](#configure), optional): Sharding settings.
  * `multi_tenancy_config` ([`Configure.multi_tenancy`](#configure), optional): Multi-tenancy settings.
  * `generative_config` ([`Configure.Generative`](#configure), optional): Generative model settings.
  * `reranker_config` ([`Configure.Reranker`](#configure), optional): Reranker settings.
* Returns: [`Collection`](#collection-object) object

#### [`create_from_dict`](#create_from_dict)
* Description: Creates a collection from a dictionary schema (e.g., JSON).
* Parameters:
  * `schema` (dict): Collection schema.
* Returns: [`Collection`](#collection-object) object

#### [`get`](#get)
* Description: Retrieves an existing collection by name.
* Parameters:
  * `name` (str): Collection name.
* Returns: [`Collection`](#collection-object) object

#### [`exists`](#exists)
* Description: Checks if a collection exists.
* Parameters:
  * `name` (str): Collection name.
* Returns: bool

#### [`list_all`](#list_all)
* Description: Lists all collections in the Weaviate instance.
* Parameters:
  * `simple` (bool, optional): If True, returns basic collection info; otherwise, full config.
* Returns: List[[`Collection`](#collection-object)] or List[dict]

#### [`delete`](#delete)
* Description: Deletes a collection by name.
* Parameters:
  * `name` (str): Collection name.

#### [`delete_all`](#delete_all)
* Description: Deletes all collections in the instance.

---

## [`Collection` Object](#collection-object)

### Attributes

#### [`config`](#config)
* Type: [`CollectionConfig`](#collectionconfig-object) object
* Description: Manages collection configuration.

#### [`data`](#data)
* Type: [`CollectionData`](#collectiondata-object) object
* Description: Manages data objects in the collection.

#### [`query`](#query)
* Type: [`CollectionQuery`](#collectionquery-object) object
* Description: Queries data in the collection.

---

## [`CollectionConfig` Object](#collectionconfig-object)

### Methods

#### [`get`](#collectionconfig-get)
* Description: Retrieves the current configuration of the collection.
* Returns: dict (Configuration details)

#### [`update`](#update)
* Description: Updates the collection configuration (e.g., vectorizer, sharding).
* Parameters:
  * `reconfigure` (`Reconfigure`): New configuration settings.

#### [`add_property`](#add_property)
* Description: Adds a new property to the collection schema.
* Parameters:
  * `property` ([`Property`](#property)): Property definition.

#### [`get_shards`](#get_shards)
* Description: Retrieves the shard status for the collection.
* Returns: List[`ShardStatus`]

#### [`update_shards`](#update_shards)
* Description: Updates shard statuses (e.g., enable/disable).
* Parameters:
  * `shards` (List[`ShardUpdate`]): Shard updates.

---

## [`CollectionData` Object](#collectiondata-object)

### Methods

#### [`insert`](#insert)
* Description: Inserts a single data object into the collection.
* Parameters:
  * `properties` (dict): Object properties.
  * `vector` (List[float], optional): Custom vector (if no vectorizer).
  * `uuid` (str, optional): Custom UUID; auto-generated if None.
  * `references` (dict, optional): Cross-references to other objects.
  * `tenant` (str, optional): Tenant name for multi-tenancy.
* Returns: str (UUID of the inserted object)

#### [`insert_many`](#insert_many)
* Description: Inserts multiple data objects in a batch.
* Parameters:
  * `objects` (List[dict]): List of objects with `properties`, `vector`, `uuid`, etc.
  * `tenant` (str, optional): Tenant name.
* Returns: List[str] (UUIDs of inserted objects)

#### [`update`](#update)
* Description: Updates an existing object’s properties or vector.
* Parameters:
  * `uuid` (str): Object UUID.
  * `properties` (dict, optional): Updated properties.
  * `vector` (List[float], optional): Updated vector.
  * `tenant` (str, optional): Tenant name.

#### [`delete`](#delete)
* Description: Deletes an object by UUID.
* Parameters:
  * `uuid` (str): Object UUID.
  * `tenant` (str, optional): Tenant name.

#### [`replace`](#replace)
* Description: Replaces an existing object entirely.
* Parameters:
  * `uuid` (str): Object UUID.
  * `properties` (dict): New properties.
  * `vector` (List[float], optional): New vector.
  * `tenant` (str, optional): Tenant name.

#### [`exists`](#exists)
* Description: Checks if an object exists by UUID.
* Parameters:
  * `uuid` (str): Object UUID.
  * `tenant` (str, optional): Tenant name.
* Returns: bool

---

## [`CollectionQuery` Object](#collectionquery-object)

### Methods

#### [`bm25`](#bm25)
* Description: Performs a BM25 keyword search.
* Parameters:
  * `query` (str): Search query string.
  * `properties` (List[str], optional): Properties to search (default: all).
  * `limit` (int, optional): Maximum number of results.
  * `offset` (int, optional): Result offset.
  * `filters` ([`Filter`](#filter), optional): Query filters.
  * `tenant` (str, optional): Tenant name.
  * `return_metadata` ([`MetadataQuery`](#metadataquery), optional): Metadata to include (e.g., distance).
* Returns: [`QueryResponse`](#queryresponse)

#### [`near_text`](#near_text)
* Description: Performs a semantic search using text input.
* Parameters:
  * `query` (str): Text to vectorize for search.
  * `distance` (float, optional): Maximum distance threshold.
  * `move_to` ([`Move`](#move), optional): Concepts to move toward.
  * `move_away` ([`Move`](#move), optional): Concepts to move away from.
  * `limit` (int, optional): Maximum results.
  * `filters` ([`Filter`](#filter), optional): Query filters.
  * `tenant` (str, optional): Tenant name.
  * `return_metadata` ([`MetadataQuery`](#metadataquery), optional): Metadata to include.
* Returns: [`QueryResponse`](#queryresponse)

#### [`near_vector`](#near_vector)
* Description: Performs a semantic search using a vector.
* Parameters:
  * `vector` (List[float]): Vector to search near.
  * `distance` (float, optional): Maximum distance threshold.
  * `limit` (int, optional): Maximum results.
  * `filters` ([`Filter`](#filter), optional): Query filters.
  * `tenant` (str, optional): Tenant name.
  * `return_metadata` ([`MetadataQuery`](#metadataquery), optional): Metadata to include.
* Returns: [`QueryResponse`](#queryresponse)

#### [`near_object`](#near_object)
* Description: Performs a semantic search near an existing object.
* Parameters:
  * `uuid` (str): UUID of the reference object.
  * `distance` (float, optional): Maximum distance threshold.
  * `limit` (int, optional): Maximum results.
  * `filters` ([`Filter`](#filter), optional): Query filters.
  * `tenant` (str, optional): Tenant name.
  * `return_metadata` ([`MetadataQuery`](#metadataquery), optional): Metadata to include.
* Returns: [`QueryResponse`](#queryresponse)

#### [`hybrid`](#hybrid)
* Description: Combines keyword (BM25) and vector search.
* Parameters:
  * `query` (str): Search query for both BM25 and vector.
  * `alpha` (float, optional): Weight between BM25 (0) and vector (1); default 0.5.
  * `limit` (int, optional): Maximum results.
  * `filters` ([`Filter`](#filter), optional): Query filters.
  * `tenant` (str, optional): Tenant name.
  * `return_metadata` ([`MetadataQuery`](#metadataquery), optional): Metadata to include.
* Returns: [`QueryResponse`](#queryresponse)

#### [`aggregate`](#aggregate)
* Description: Performs aggregation queries (e.g., count, average) on collection data.
* Parameters:
  * `filters` ([`Filter`](#filter), optional): Filters for aggregation.
  * `group_by` (str, optional): Property to group by.
  * `tenant` (str, optional): Tenant name.
* Returns: dict (Aggregation results)

#### [`fetch_object_by_id`](#fetch_object_by_id)
* Description: Retrieves an object by its UUID.
* Parameters:
  * `uuid` (str): Object UUID.
  * `include_vector` (bool or List[str], optional): Include vector(s) in response.
  * `tenant` (str, optional): Tenant name.
* Returns: [`DataObject`](#dataobject)

---

## [`classes` Module](#classes-module)

### [`config` Submodule](#config-submodule)

#### [`Configure`](#configure)
* **[`Vectorizer`](#configure)**  
  * Methods:
    * `text2vec_openai(api_key=None)`: OpenAI vectorizer.
    * `text2vec_cohere(api_key=None)`: Cohere vectorizer.
    * `text2vec_huggingface(model=None)`: Hugging Face vectorizer.
    * `none()`: No vectorizer.
* **[`VectorIndex`](#configure)**  
  * Methods:
    * `hnsw(distance_metric=VectorDistances.COSINE)`: HNSW index.
    * `flat()`: Flat index.
    * `dynamic()`: Dynamic index.
* **[`inverted_index`](#configure)**  
  * Methods: `inverted_index(bm25_k1=1.2, bm25_b=0.75)`: Configures BM25 parameters.
* **[`replication`](#configure)**  
  * Methods: `replication(factor=1)`: Sets replication factor.
* **[`sharding`](#configure)**  
  * Methods: `sharding(shard_count=1)`: Sets shard count.
* **[`multi_tenancy`](#configure)**  
  * Methods: `multi_tenancy(enabled=False)`: Enables multi-tenancy.
* **[`Generative`](#configure)**  
  * Methods:
    * `openai(api_key=None)`: OpenAI generative model.
    * `cohere(api_key=None)`: Cohere generative model.
* **[`Reranker`](#configure)**  
  * Methods: `cohere(api_key=None)`: Cohere reranker.

#### [`Property`](#property)
* Description: Defines a collection property.
* Parameters:
  * `name` (str): Property name.
  * `data_type` ([`DataType`](#datatype)): Data type (e.g., TEXT, INT).

#### [`DataType`](#datatype)
* Description: Enum-like class for property data types (e.g., `TEXT`, `INT`, `NUMBER`, `BOOLEAN`, `DATE`).

#### [`Tokenization`](#tokenization)
* Description: Defines tokenization strategies (e.g., `word`, `lowercase`).

#### [`VectorDistances`](#vectordistances)
* Description: Defines vector distance metrics (e.g., `COSINE`, `L2`, `DOT`).

### [`query` Submodule](#query-submodule)

#### [`Move`](#move)
* Description: Specifies concepts to move toward/away in semantic search.
* Parameters:
  * `concepts` (List[str]): Concepts to move.
  * `force` (float): Movement strength.

#### [`MetadataQuery`](#metadataquery)
* Description: Specifies metadata to return in queries (e.g., `distance=True`, `score=True`).

#### [`Filter`](#filter)
* Description: Defines query filters.
* Methods: `by_property(name).equal(value)`, `by_property(name).greater_than(value)`, etc.

### [`init` Submodule](#init-submodule)

#### [`Auth`](#auth)
* Description: Authentication methods.
* Methods:
  * `api_key(api_key)`: API key authentication.

#### [`Timeout`](#timeout)
* Description: Configures request timeouts.
* Parameters:
  * `request` (float): Request timeout in seconds.
  * `connection` (float): Connection timeout in seconds.

### [`data` Submodule](#data-submodule)

#### [`DataObject`](#dataobject)
* Description: Represents a retrieved data object.
* Attributes:
  * `properties` (dict): Object properties.
  * `vector` (List[float]): Object vector.
  * `metadata` (dict): Metadata (e.g., distance).
  * `uuid` (str): Object UUID.

#### [`QueryResponse`](#queryresponse)
* Description: Represents a query response.
* Attributes:
  * `objects` (List[[`DataObject`](#dataobject)]): Retrieved objects.
  * `errors` (List[dict]): Query errors, if any.

#### [`Reference`](#reference)
* Description: Defines cross-references between objects.
* Parameters:
  * `to` (str): UUID of the target object.
  * `property` (str): Reference property name.