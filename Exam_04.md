# Data Management — Practice Exam 4

20 questions · multiple choice (one correct answer) · based on Ch-8, Ch-9, Ch-10, Ch-14

---

**1. The HTTP protocol is:**

- A) Stateful by default
- B) Connectionless
- C) A scripting language
- D) A storage engine

**2. A B+-tree leaf node has between:**

- A) n/2 and n values
- B) 2 and n children
- C) 0 and n pointers
- D) (n-1)/2 and n-1 values

**3. GIS stands for:**

- A) Geometric Integrity System
- B) Geographic Information Systems
- C) Generalized Indexing Scheme
- D) Global Index Storage

**4. Term Frequency is defined as TF(d,t) =**

- A) log(1 + n(d,t)/n(d))
- B) 1 / n(t)
- C) n(t) * n(d)
- D) n(d)/n(d,t)

**5. The CGI standard defines:**

- A) How B+-trees are split
- B) How browsers render HTML
- C) How the web server communicates with application programs
- D) How cookies are encrypted

**6. A hash function h maps:**

- A) Search-key values to bucket addresses
- B) Bucket addresses to disk blocks
- C) Records to reduce keys
- D) Tags to values

**7. Big Data is commonly contrasted with traditional databases along three Vs:**

- A) Vector, Velocity, Variety
- B) Volume, Velocity, Variety
- C) Volume, Vertex, Value
- D) Volume, Value, Veracity

**8. To create a servlet, the programmer writes a class that inherits from:**

- A) JSPPage
- B) HttpServlet
- C) CGIScript
- D) HttpSession

**9. The two basic kinds of indices are:**

- A) Primary indices and cookies
- B) Dense indices and bitmap indices
- C) Vector and raster indices
- D) Ordered indices and hash indices

**10. In the 3 Vs, Variety refers to:**

- A) The size of each record
- B) The number of replicas
- C) Many types of data beyond relational data
- D) The growth rate of inserts

**11. Key-value stores are not full database systems because they:**

- A) Cannot store any records
- B) Do not allow lookups by key
- C) Have no or limited support for transactional updates
- D) Require SQL for every query

**12. Secondary (non-clustering) indices must be:**

- A) In-memory only
- B) Multilevel
- C) Dense
- D) Sparse

**13. A drawback of the basic B+-tree structure is that performance can be poor with:**

- A) In-memory lookups
- B) Random writes/inserts
- C) Sequential reads
- D) Range queries

**14. In a servlet container, each HTTP request:**

- A) Creates a new database
- B) Opens a permanent connection
- C) Spawns a new thread that is closed once the request is serviced
- D) Reuses one global thread forever

**15. Key-value storage systems are often also called:**

- A) NoSQL systems
- B) Buffer trees
- C) GIS databases
- D) OLAP cubes

**16. XML marks up information using:**

- A) Fixed-length binary fields
- B) Curly braces around key-value pairs
- C) Comma-separated values
- D) Tags enclosed in angle brackets

**17. Skew in the distribution of records can occur because:**

- A) The index is too small
- B) Multiple records have the same search-key value or the hash function is non-uniform
- C) The B+-tree is unbalanced
- D) Records are sequentially ordered

**18. In a two-level index, the outer index is:**

- A) The leaf level of a B+-tree
- B) A sparse index of the basic (inner) index
- C) A dense copy of all records
- D) A hash table of buckets

**19. An LSM tree consists of:**

- A) One bitmap per attribute
- B) A chain of overflow buckets
- C) Several B+-trees: an in-memory L0 and on-disk L1..Lk
- D) A single hash table

**20. Inverse document frequency is defined as IDF(t) =**

- A) n(t) / n(d)
- B) 1 / n(t)
- C) log(1 + n(d,t))
- D) n(d,t) * n(d)

---

## Answer Key

1-B  2-D  3-B  4-A  5-C  6-A  7-B  8-B  9-D  10-C  11-C  12-C  13-B  14-C  15-A  16-D  17-B  18-B  19-C  20-B
