# Data Management — Practice Exam 7

20 questions · multiple choice (one correct answer) · based on Ch-8, Ch-9, Ch-10, Ch-14

---

**1. A JSP page is ultimately:**

- A) Run inside the browser
- B) Compiled into Java + servlet code
- C) Stored as a cookie
- D) Executed as raw SQL

**2. In a two-layer web architecture:**

- A) Each request starts a new process via CGI
- B) There is no database
- C) The web server and application server are combined into a single server
- D) The client connects directly to the database

**3. The result of 100110 OR 110011 is:**

- A) 000010
- B) 011001
- C) 110111
- D) 100010

**4. A clustering index (primary index) is one whose search key:**

- A) Points to overflow buckets
- B) Differs from the sequential order of the file
- C) Is always non-unique
- D) Specifies the sequential order of the file

**5. In a wide column representation, each tuple:**

- A) Can have a different set of attributes
- B) Cannot add new attributes
- C) Stores only numeric values
- D) Must share one fixed schema

**6. In a B+-tree, all paths from the root to a leaf are:**

- A) Of the same length
- B) Equal to the number of attributes
- C) Of different lengths
- D) At most 2 nodes long

**7. Information retrieval generally refers to the querying of:**

- A) Geometric design data
- B) Unstructured textual data
- C) Hash buckets
- D) Normalized relational data

**8. Key-value stores typically support which operations?**

- A) map, reduce, shuffle
- B) crawl, rank, retrieve
- C) put(key,value), get(key), delete(key)
- D) select, project, join

**9. GFS stands for:**

- A) Globalファイル System
- B) Grid File Service
- C) Generalized File Storage
- D) Google File System

**10. In a bitmap for value v, the bit for a record is 1 if:**

- A) The attribute is null
- B) The record has the value v for the attribute
- C) The record was recently inserted
- D) The record is the first in its bucket

**11. The CGI standard defines:**

- A) How B+-trees are split
- B) How the web server communicates with application programs
- C) How cookies are encrypted
- D) How browsers render HTML

**12. In the 3 Vs, Velocity refers to:**

- A) Much higher rates of data insertion/arrival
- B) The number of data types
- C) The speed of disk seeks only
- D) The total amount of stored data

**13. The presentation/user-interface layer is itself often broken up based on the:**

- A) Three-V model
- B) B+-tree structure
- C) MapReduce paradigm
- D) Model-View-Controller (MVC) architecture

**14. A key deficiency of static hashing is that:**

- A) It wastes no space ever
- B) It cannot perform lookups
- C) It requires a B+-tree
- D) The set of bucket addresses is fixed while the database grows or shrinks

**15. A polygon is represented by:**

- A) A single (x,y) point
- B) An ordered list of vertices
- C) A key-value map
- D) A pair of endpoints

**16. Hadoop, the widely used open-source MapReduce implementation, is written in:**

- A) Java
- B) Go
- C) C++
- D) Python

**17. A dense index has:**

- A) An entry for only some search-key values
- B) One entry per disk block
- C) No pointers
- D) An index entry for every search-key value in the file

**18. XQuery was developed to:**

- A) Hash search keys into buckets
- B) Partition data across shards
- C) Query nested XML structures
- D) Build B+-tree indices

**19. A benefit of the LSM approach is that inserts are done using:**

- A) No I/O at all
- B) Only sequential I/O operations
- C) Only random I/O operations
- D) Bitmap intersections

**20. To locate a record value K using a sparse index you:**

- A) Use NOT on a bitmap
- B) Find the index record with the largest search-key value < K, then scan sequentially
- C) Hash K to a bucket
- D) Read every record in the file

---

## Answer Key

1-B  2-C  3-C  4-D  5-A  6-A  7-B  8-C  9-D  10-B  11-B  12-A  13-D  14-D  15-B  16-A  17-D  18-C  19-B  20-B
