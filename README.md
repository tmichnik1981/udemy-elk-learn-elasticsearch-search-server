# ELK Learn Elasticsearch Search Server

#### ES Intro

- Core - Lucene
  - Searching and Indexing
  - Document Analysis
  - Quering
  - Result
- EL extra features
  - Fast indexing
  - Real time search
  - Low latency and faster query respose
  - Data avalilability
- Resiliency
- Distributable and scalability
- Efficiency and speed to indexing
- Detailed diagnostics and instrumentation
- Nodes:
  - Master - controls the cluster
  - Data - create, read, update, delete, search, agregration
  - Client - smart router, forward cluster level requests to master node and data requests to data nodes
- Zen Discovery
- Shard - low level worker unit, holds one slice of all the data in the index, single instance of lucene
- Multitenency
  - primary shards
  - replica shards
- Availability
  - Node are replicated
  - If one is down, traffic is directed to another node
- Extensibility
  - plugins
  - tools
  - schef and puppet recipies
  - Site plugins
    - Head
    - BigDesk
    - Kopt
  - Analysis Plugins
    - SmartCN
  - Percolation
    - Queries are registered

##### Key Components

- Cluster
  - Collection of nodes
  - Identified by a unique
- Node
  - Single server
  - Identified by a random Id
  - Master Node
    - Light weight node
    - Responsible for cluster management
    - Ensures the cluster is stable
    - It is not recommended to send index or search request to this node
  - Data Node
    - Storing actual data
    - Participates in indexing process
  - Client Node
    - Acts as a load balancer for processing requests
    - Used to perform scatter/gather based operations like search
    - Neither stores the data nor participates in cluster management
    - Relieves data node to od heavy duty of searching
- Document
  - JSON document stored in ElasticSearch
  - Equivalent to a row in a relational database
- Index
  - Collection of document having similar characteristics
  - Equivalent to a database instance in a relational database world
  - Mappings which defines multiple types
  - Logical namespace to map one or more primary shards
  - Can have Zero or more replicas
  - Supports time-series indexing
- Type
  - Equivalent to table in a relational database
  - Each type has a list of fields
  - Mapping defines how each field is analyzed
- ID
  - unique identifier to identify a document 
  - Combination of index, type and id must be unique to be able to identify a document deterministically
  - The user can specify the id but id can also be auto generated by ElasticSearch if it is not provided 
- Shard
  - Locical unit to store data
  - Each document is stored in single promary shard
  - By default each index has 5 shards
  - Number of shards cannot be changed ater index creation
- Replicas
  - Each index can have 0 or more replicas
  - Helps with fail over, performance
  - Replicas are never stored on the same node as promary shard node
  - Can be changed after index creation
- Node Failures 
  - Achieved by replicas
  - Provides high availability to the cluster
- Mapping
  - Equivalent to schema in relational database
  - Each index has mappings which define each type within a index
  - Can be defined explicitly or it will be generated automatically
- Analysis
  - Process of converting full text to terms
  - Texts are broken depending upon type of analyzers
  - E.g. super-fast => super and fast
- Snapshots and Restore
  - Used for index backup and restore
  - Supports full or incremental index backpup

#### ElasticSearch in Action

##### Comparison to RDBMS

| Elastic Search        | RDMBS                                  |
| --------------------- | -------------------------------------- |
| Index                 | Database                               |
| Type                  | Table                                  |
| Document              | Row                                    |
| Field                 | Column                                 |
| Mapping               | Schema                                 |
| Query DSL             | SQL                                    |
| Get API               | SELECT * FROM                          |
| POST API              | UPDATE <TABLE> SET                     |
| PUT API               | INSERT INTO <TABLE>                    |
| Everything is indexed | Index on demand                        |
| Aggregation           | SELECT  <FIELD>, COUNT(*) FROM <TABLE> |

##### Features

- Indexing and searching happens in near real time
- Support for bulk and incremental indexing
- Time series indexing for time based streams of data
- Schema free
- Everything is one JSON call away
- Support for nested documents
- Inverted indexing
- Custom analyzers and on the fly analyzer selection
- Keyword and Metadata based search
- Enrichment
- Sorting
- Support for advanced search features

##### Installation

- [Reference](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [Download and install](https://www.elastic.co/downloads/elasticsearch)

##### Installation on EC2

1. Go to Services EC2
   - Go to Instances
   - Click Launch
2. Select Ubuntu
   - Select t2.micro (free tier)
3. Number of instances :2
4. Storage: leave default
5. Tag instance: leave default
6. Configure Security Group
   - Security group name: Tetra_Search_Demo
   - Type: All traffic, Protocol: All, Port Range: All
   - Use existing or create new key pair
   - Download .PEM certificate
7. Launch instances



##### The Default Path of Installation

| Type    | Description                                                  | Location Debian/Ubuntu           | location RHEL, CentOS            |
| ------- | ------------------------------------------------------------ | -------------------------------- | -------------------------------- |
| Home    | Home of ES instalation                                       | /usr/share/elasticsearch         | /usr/share/elasticsearch         |
| Bin     | Binary scripts including es to start a node                  | /usr/share/elasticsearch/bin     | /usr/share/elasticsearch/bin     |
| Conf    | Configuration file elasticsearch.yml and logging.yml         | /etc/elasticsearch               | /etc/elasticsearch               |
| data    | The location of the data files of each index or shard allocated on the node | /var/lib/elasticsearch/data      | /var/lib/elasticsearch           |
| logs    | Log file location                                            | /var/log/elasticsearch           | /var/log/elasticsearch           |
| plugins | plugin file location. Each plugin will be contained in a subdirectory. | /usr/share/elasticsearch/plugins | /usr/share/elasticsearch/plugins |

##### The Configuration Changes

| Parameter name           | Parameter value |
| ------------------------ | --------------- |
| cluster.name             | TetraNoodles    |
| node.name                | Tetra_search_1  |
| index.number_of_shards   | 5               |
| index.number_of_replicas | 1               |
| node.master              | true            |
| node.data                | false           |

###### Discovery 

discovery.zen.ping.unicast.hosts: ["172.31.28.121", "172.31.28.122"]

discovery.zen.ping.multicast.enabled: false

###### Network

network.host: 172.31.28.121