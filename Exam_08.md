# Data Management — Practice Exam 8

20 questions · multiple choice (one correct answer) · based on Ch-8, Ch-9, Ch-10, Ch-14

---

**1. Deletion in an LSM tree is handled by:**

- A) Dropping the whole L0 tree
- B) Immediately rewriting all levels
- C) Splitting a leaf node
- D) Adding special 'delete' entries

**2. Key-value stores are not full database systems because they:**

- A) Have no or limited support for transactional updates
- B) Require SQL for every query
- C) Do not allow lookups by key
- D) Cannot store any records

**3. The two basic kinds of indices are:**

- A) Dense indices and bitmap indices
- B) Primary indices and cookies
- C) Ordered indices and hash indices
- D) Vector and raster indices

**4. The Web era of application architecture began in the:**

- A) 1980s
- B) 2020s
- C) 1960s
- D) Mid 1990s

**5. The CGI standard defines:**

- A) How browsers render HTML
- B) How the web server communicates with application programs
- C) How cookies are encrypted
- D) How B+-trees are split

**6. SPARQL is a query language designed to query:**

- A) Bitmap indices
- B) RDF data
- C) Spatial rasters
- D) Relational tables

**7. In a two-layer web architecture:**

- A) Each request starts a new process via CGI
- B) There is no database
- C) The client connects directly to the database
- D) The web server and application server are combined into a single server

**8. A pointer to a record consists of:**

- A) A reduce key and a count
- B) Only the search-key value
- C) The identifier of a disk block and an offset within the block
- D) A cookie and a session ID

**9. The type information of an object in RDF is provided by the:**

- A) instance-of relationship
- B) hash function
- C) primary key
- D) DOM tree

**10. Compressed input formats supported by Hadoop include:**

- A) JPEG, PNG and GIF
- B) MP3, WAV and FLAC
- C) DOCX, PPTX and XLSX
- D) Avro, ORC and Parquet

**11. In MVC, the controller:**

- A) Compiles HTML to bytecode
- B) Replicates blocks across nodes
- C) Receives events, executes actions on the model, and returns a view to the user
- D) Stores data on disk

**12. To locate a record value K using a sparse index you:**

- A) Find the index record with the largest search-key value < K, then scan sequentially
- B) Use NOT on a bitmap
- C) Hash K to a bucket
- D) Read every record in the file

**13. Inverse document frequency is defined as IDF(t) =**

- A) n(t) / n(d)
- B) log(1 + n(d,t))
- C) 1 / n(t)
- D) n(d,t) * n(d)

**14. An LSM tree consists of:**

- A) One bitmap per attribute
- B) A single hash table
- C) A chain of overflow buckets
- D) Several B+-trees: an in-memory L0 and on-disk L1..Lk

**15. An index file consists of records (index entries) of the form:**

- A) key and bucket count
- B) search-key and pointer
- C) subject and predicate
- D) tag and value

**16. Bitmap indices are particularly useful for:**

- A) Single-attribute equality only
- B) Selections on multiple keys
- C) Range queries on a B+-tree
- D) Spatial joins

**17. A frequently cited drawback of JSON is that it is:**

- A) Unable to represent arrays
- B) Limited to integers only
- C) Verbose and CPU-intensive to parse
- D) Not usable for data exchange

**18. The key idea of a buffer tree is to:**

- A) Replace leaves with hash buckets
- B) Store data only in memory
- C) Associate a buffer with each internal node of a B+-tree
- D) Remove all internal nodes

**19. In a distributed file system, each file block is typically replicated across:**

- A) All machines
- B) One machine
- C) Three machines
- D) Ten machines

**20. Parallel databases (as opposed to MapReduce systems) were developed in the:**

- A) 1980s
- B) 2020s
- C) 2010s
- D) 1960s

---

## Answer Key

1-D  2-A  3-C  4-D  5-B  6-B  7-D  8-C  9-A  10-D  11-C  12-A  13-C  14-D  15-B  16-B  17-C  18-C  19-C  20-A
