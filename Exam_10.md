# Data Management — Practice Exam 10

20 questions · multiple choice (one correct answer) · based on Ch-8, Ch-9, Ch-10, Ch-14

---

**1. In the JavaScript example, the validate() function rejects credits unless they are:**

- A) Greater than 16
- B) Any text value
- C) Equal to zero
- D) A number greater than 0 and less than 16

**2. In HDFS, a DataNode maps a:**

- A) Search key to a pointer
- B) Block ID to a physical location on disk
- C) Key to a bucket address
- D) Filename to a list of Block IDs

**3. Buffer trees are used in:**

- A) Amazon DynamoDB
- B) PostgreSQL Generalized Search Tree (GiST) indices
- C) Hadoop MapReduce
- D) Google File System

**4. In a two-layer web architecture:**

- A) Each request starts a new process via CGI
- B) There is no database
- C) The client connects directly to the database
- D) The web server and application server are combined into a single server

**5. One of the key requirements of the core relational model is that data values be:**

- A) Atomic
- B) Multivalued
- C) Hierarchical
- D) Composite

**6. Deletion in an LSM tree is handled by:**

- A) Dropping the whole L0 tree
- B) Adding special 'delete' entries
- C) Immediately rewriting all levels
- D) Splitting a leaf node

**7. A node split during B+-tree insertion propagates:**

- A) Upward toward the root
- B) Downward to the leaves
- C) Left to the sibling
- D) Nowhere

**8. In a distributed file system, each file block is typically replicated across:**

- A) Ten machines
- B) One machine
- C) Three machines
- D) All machines

**9. GFS stands for:**

- A) Google File System
- B) Generalized File Storage
- C) Globalファイル System
- D) Grid File Service

**10. The RDF model represents data as:**

- A) Pixel maps
- B) A balanced tree of nodes
- C) Fixed relational tables
- D) A set of triples

**11. Which two data models are widely used for semi-structured data?**

- A) RDF and SQL
- B) BSON and Avro
- C) CSV and YAML
- D) JSON and XML

**12. Storing readings as the array [5, 8, 9, 11] instead of (time, value) pairs works because:**

- A) The position (offset) encodes the time, so time need not be stored
- B) Arrays cannot store time
- C) Each value is hashed
- D) Time is irrelevant to all queries

**13. In a B+-tree leaf node, the pointer Pn points to:**

- A) A record bucket
- B) The root
- C) The parent node
- D) The next leaf node in search-key order

**14. In a URL, the prefix 'http' indicates that the document is accessed using:**

- A) A secure shell
- B) The HyperText Transfer Protocol
- C) A hash function
- D) A distributed file system

**15. In a two-level index, the outer index is:**

- A) A sparse index of the basic (inner) index
- B) A hash table of buckets
- C) The leaf level of a B+-tree
- D) A dense copy of all records

**16. Key-value stores are not full database systems because they:**

- A) Have no or limited support for transactional updates
- B) Require SQL for every query
- C) Cannot store any records
- D) Do not allow lookups by key

**17. The result of 100110 OR 110011 is:**

- A) 100010
- B) 110111
- C) 011001
- D) 000010

**18. The presentation/user-interface layer is itself often broken up based on the:**

- A) MapReduce paradigm
- B) Three-V model
- C) Model-View-Controller (MVC) architecture
- D) B+-tree structure

**19. A drawback of the LSM approach is that:**

- A) It cannot handle deletions
- B) Queries have to search multiple trees
- C) Leaves are always half empty
- D) Inserts require random I/O

**20. A non-leaf, non-root B+-tree node has between:**

- A) (n-1)/2 and n-1 children
- B) n/2 and n children
- C) 2 and 3 children
- D) 1 and n values

---

## Answer Key

1-D  2-B  3-B  4-D  5-A  6-B  7-A  8-C  9-A  10-D  11-D  12-A  13-D  14-B  15-A  16-A  17-B  18-C  19-B  20-B
