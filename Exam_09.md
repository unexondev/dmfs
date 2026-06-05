# Data Management — Practice Exam 9

20 questions · multiple choice (one correct answer) · based on Ch-8, Ch-9, Ch-10, Ch-14

---

**1. In HDFS, a DataNode maps a:**

- A) Search key to a pointer
- B) Block ID to a physical location on disk
- C) Key to a bucket address
- D) Filename to a list of Block IDs

**2. When a full B+-tree leaf is split on insertion, the first ceil(n/2) entries:**

- A) Are hashed to buckets
- B) Move to the parent
- C) Stay in the original node and the rest go to a new node
- D) Are deleted

**3. The result of NOT 100110 is:**

- A) 111111
- B) 100010
- C) 011001
- D) 110111

**4. Secondary (non-clustering) indices must be:**

- A) In-memory only
- B) Multilevel
- C) Sparse
- D) Dense

**5. A JSP page is ultimately:**

- A) Executed as raw SQL
- B) Stored as a cookie
- C) Run inside the browser
- D) Compiled into Java + servlet code

**6. A polygon is represented by:**

- A) A single (x,y) point
- B) An ordered list of vertices
- C) A key-value map
- D) A pair of endpoints

**7. With K search-key values, the height of a B+-tree is at most:**

- A) n * K
- B) log base 2 of K
- C) K / n
- D) log base ceil(n/2) of K

**8. Unlike the E-R model, the RDF model supports only:**

- A) Ternary relationships
- B) Multivalued attributes
- C) Binary relationships
- D) Composite keys

**9. AJAX technology allows a page to:**

- A) Replace HTTP with SSH
- B) Compile servlets in the browser
- C) Shard a database
- D) Fetch data from a server and update the page without reloading

**10. MongoDB and CouchDB are document stores that typically store:**

- A) Semi-structured JSON data
- B) B-tree leaf pages
- C) Bitmap indices
- D) Raster images

**11. Term Frequency is defined as TF(d,t) =**

- A) 1 / n(t)
- B) n(d)/n(d,t)
- C) n(t) * n(d)
- D) log(1 + n(d,t)/n(d))

**12. In the term B+-tree, the B stands for:**

- A) Balanced
- B) Bucket
- C) Block
- D) Binary

**13. In HDFS, the master server that manages the file system namespace is the:**

- A) DataNode
- B) JobTracker
- C) Controller
- D) NameNode

**14. An LSM tree consists of:**

- A) Several B+-trees: an in-memory L0 and on-disk L1..Lk
- B) One bitmap per attribute
- C) A single hash table
- D) A chain of overflow buckets

**15. In MVC, the controller:**

- A) Receives events, executes actions on the model, and returns a view to the user
- B) Replicates blocks across nodes
- C) Stores data on disk
- D) Compiles HTML to bytecode

**16. Consistency of replicated data is often implemented using:**

- A) CGI
- B) Majority protocols
- C) Hash chaining
- D) Bitmap intersection

**17. In a hash index vs a hash file-organization:**

- A) Both store only records
- B) Both store only pointers
- C) A hash index stores pointers to records; a hash file-organization stores the records themselves
- D) Neither uses a hash function

**18. Recall measures:**

- A) What percentage of returned results are relevant
- B) The depth of a B+-tree
- C) What percentage of relevant results were returned
- D) The number of distinct terms

**19. Bitmap indices are particularly useful for:**

- A) Selections on multiple keys
- B) Single-attribute equality only
- C) Range queries on a B+-tree
- D) Spatial joins

**20. The presentation/user-interface layer is itself often broken up based on the:**

- A) B+-tree structure
- B) Model-View-Controller (MVC) architecture
- C) MapReduce paradigm
- D) Three-V model

---

## Answer Key

1-B  2-C  3-C  4-D  5-D  6-B  7-D  8-C  9-D  10-A  11-D  12-A  13-D  14-A  15-A  16-B  17-C  18-C  19-A  20-B
