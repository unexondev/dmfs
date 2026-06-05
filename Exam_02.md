# Data Management — Practice Exam 2

20 questions · multiple choice (one correct answer) · based on Ch-8, Ch-9, Ch-10, Ch-14

---

**1. In a hash index vs a hash file-organization:**

- A) Neither uses a hash function
- B) Both store only pointers
- C) Both store only records
- D) A hash index stores pointers to records; a hash file-organization stores the records themselves

**2. Hadoop, the widely used open-source MapReduce implementation, is written in:**

- A) Go
- B) C++
- C) Java
- D) Python

**3. In MVC, the controller:**

- A) Compiles HTML to bytecode
- B) Replicates blocks across nodes
- C) Stores data on disk
- D) Receives events, executes actions on the model, and returns a view to the user

**4. ST_Contains() and ST_Overlaps() in PostGIS are examples of:**

- A) Bitmap operations
- B) Servlet methods
- C) Spatial / region query functions
- D) Hash functions

**5. Parallel databases (as opposed to MapReduce systems) were developed in the:**

- A) 1980s
- B) 2010s
- C) 2020s
- D) 1960s

**6. In HDFS, the master server that manages the file system namespace is the:**

- A) Controller
- B) NameNode
- C) DataNode
- D) JobTracker

**7. A key deficiency of static hashing is that:**

- A) It wastes no space ever
- B) It cannot perform lookups
- C) It requires a B+-tree
- D) The set of bucket addresses is fixed while the database grows or shrinks

**8. In a two-level index, the outer index is:**

- A) The leaf level of a B+-tree
- B) A dense copy of all records
- C) A hash table of buckets
- D) A sparse index of the basic (inner) index

**9. A composite attribute such as name(firstname, lastname) leads to the use of the term:**

- A) Raster data
- B) Sparse columns
- C) Nested data types
- D) Atomic types

**10. A JSP page is ultimately:**

- A) Executed as raw SQL
- B) Stored as a cookie
- C) Run inside the browser
- D) Compiled into Java + servlet code

**11. When a full B+-tree leaf is split on insertion, the first ceil(n/2) entries:**

- A) Stay in the original node and the rest go to a new node
- B) Are hashed to buckets
- C) Are deleted
- D) Move to the parent

**12. The RDF model represents data as:**

- A) Fixed relational tables
- B) A balanced tree of nodes
- C) A set of triples
- D) Pixel maps

**13. BSON is best described as:**

- A) An XML query language
- B) A binary, compressed representation of JSON for efficient storage
- C) A relational normalization form
- D) A spatial coordinate system

**14. A non-leaf, non-root B+-tree node has between:**

- A) n/2 and n children
- B) (n-1)/2 and n-1 children
- C) 1 and n values
- D) 2 and 3 children

**15. The Web era of application architecture began in the:**

- A) 2020s
- B) Mid 1990s
- C) 1980s
- D) 1960s

**16. In the MapReduce paradigm, the programmer provides:**

- A) The replication protocol
- B) The map() and reduce() functions
- C) The shuffle and sort code
- D) The disk scheduler

**17. A pointer to a record consists of:**

- A) A cookie and a session ID
- B) The identifier of a disk block and an offset within the block
- C) A reduce key and a count
- D) Only the search-key value

**18. In the MVC architecture, the business-logic layer corresponds to the:**

- A) Controller
- B) Model
- C) View
- D) Cookie

**19. A benefit of the LSM approach is that inserts are done using:**

- A) Only sequential I/O operations
- B) No I/O at all
- C) Bitmap intersections
- D) Only random I/O operations

**20. A drawback of the LSM approach is that:**

- A) It cannot handle deletions
- B) Queries have to search multiple trees
- C) Leaves are always half empty
- D) Inserts require random I/O

---

## Answer Key

1-D  2-C  3-D  4-C  5-A  6-B  7-D  8-D  9-C  10-D  11-A  12-C  13-B  14-A  15-B  16-B  17-B  18-B  19-A  20-B
