# Data Management — Practice Exam 6

20 questions · multiple choice (one correct answer) · based on Ch-8, Ch-9, Ch-10, Ch-14

---

**1. The key idea of a buffer tree is to:**

- A) Replace leaves with hash buckets
- B) Remove all internal nodes
- C) Store data only in memory
- D) Associate a buffer with each internal node of a B+-tree

**2. With K search-key values, the height of a B+-tree is at most:**

- A) n * K
- B) K / n
- C) log base 2 of K
- D) log base ceil(n/2) of K

**3. When a full B+-tree leaf is split on insertion, the first ceil(n/2) entries:**

- A) Stay in the original node and the rest go to a new node
- B) Move to the parent
- C) Are deleted
- D) Are hashed to buckets

**4. XQuery was developed to:**

- A) Partition data across shards
- B) Build B+-tree indices
- C) Query nested XML structures
- D) Hash search keys into buckets

**5. SPARQL is a query language designed to query:**

- A) Spatial rasters
- B) Relational tables
- C) RDF data
- D) Bitmap indices

**6. Which of these are server-side scripting languages?**

- A) C and Assembly
- B) SPARQL and XQuery
- C) JSP and PHP
- D) HTML and CSS

**7. An LSM tree consists of:**

- A) One bitmap per attribute
- B) Several B+-trees: an in-memory L0 and on-disk L1..Lk
- C) A single hash table
- D) A chain of overflow buckets

**8. MongoDB and CouchDB are document stores that typically store:**

- A) B-tree leaf pages
- B) Raster images
- C) Semi-structured JSON data
- D) Bitmap indices

**9. Compared with fetching tuples, counting the number of matching tuples using bitmaps is:**

- A) Much slower
- B) Even faster
- C) The same cost
- D) Impossible

**10. Key-value stores are not full database systems because they:**

- A) Cannot store any records
- B) Do not allow lookups by key
- C) Have no or limited support for transactional updates
- D) Require SQL for every query

**11. AJAX technology allows a page to:**

- A) Replace HTTP with SSH
- B) Fetch data from a server and update the page without reloading
- C) Shard a database
- D) Compile servlets in the browser

**12. A clustering index (primary index) is one whose search key:**

- A) Is always non-unique
- B) Specifies the sequential order of the file
- C) Points to overflow buckets
- D) Differs from the sequential order of the file

**13. In HDFS, the master server that manages the file system namespace is the:**

- A) Controller
- B) DataNode
- C) JobTracker
- D) NameNode

**14. The shuffle step in MapReduce essentially performs a:**

- A) Bitmap OR operation
- B) Binary search on the index
- C) Group by on the reduce key across machines
- D) Join of two relations

**15. In a servlet container, each HTTP request:**

- A) Reuses one global thread forever
- B) Spawns a new thread that is closed once the request is serviced
- C) Opens a permanent connection
- D) Creates a new database

**16. Which set of operations is defined on a key-value map?**

- A) crawl, rank, index
- B) insert, select, drop
- C) split, merge, redistribute
- D) put(key,value), get(key), delete(key)

**17. A benefit of the LSM approach is that inserts are done using:**

- A) Only sequential I/O operations
- B) No I/O at all
- C) Bitmap intersections
- D) Only random I/O operations

**18. Information retrieval generally refers to the querying of:**

- A) Geometric design data
- B) Normalized relational data
- C) Unstructured textual data
- D) Hash buckets

**19. In the MVC architecture, the business-logic layer corresponds to the:**

- A) View
- B) Model
- C) Controller
- D) Cookie

**20. In the term B+-tree, the B stands for:**

- A) Bucket
- B) Balanced
- C) Binary
- D) Block

---

## Answer Key

1-D  2-D  3-A  4-C  5-C  6-C  7-B  8-C  9-B  10-C  11-B  12-B  13-D  14-C  15-B  16-D  17-A  18-C  19-B  20-B
