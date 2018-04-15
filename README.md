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

