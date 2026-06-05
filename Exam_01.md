# Data Management — Practice Exam 1

20 questions · multiple choice (one correct answer) · based on Ch-8, Ch-9, Ch-10, Ch-14

---

**1. In a two-layer web architecture:**

- A) Each request starts a new process via CGI
- B) The web server and application server are combined into a single server
- C) The client connects directly to the database
- D) There is no database

**2. A commonly used partitioning (shard) key is:**

- A) Reduce count
- B) Disk block offset
- C) Cookie value
- D) User ID

**3. Compressed input formats supported by Hadoop include:**

- A) DOCX, PPTX and XLSX
- B) MP3, WAV and FLAC
- C) Avro, ORC and Parquet
- D) JPEG, PNG and GIF

**4. The type information of an object in RDF is provided by the:**

- A) DOM tree
- B) hash function
- C) instance-of relationship
- D) primary key

**5. A disadvantage of indexed-sequential files is that:**

- A) Performance degrades as the file grows due to overflow blocks
- B) They support only range queries
- C) They cannot store pointers
- D) They require a hash function

**6. Secondary indices on a non-unique search key typically point to:**

- A) The root of the B+-tree
- B) A bucket that contains pointers to the actual records
- C) A single record only
- D) An overflow chain of leaves

**7. Key-value stores are not full database systems because they:**

- A) Do not allow lookups by key
- B) Cannot store any records
- C) Have no or limited support for transactional updates
- D) Require SQL for every query

**8. A search key that contains more than one attribute is called a:**

- A) Composite search key
- B) Foreign key
- C) Candidate key
- D) Hash key

**9. The result of 100110 OR 110011 is:**

- A) 011001
- B) 100010
- C) 000010
- D) 110111

**10. Bucket overflow is handled by using:**

- A) Overflow buckets chained in a linked list
- B) Binary search
- C) Replication across 3 nodes
- D) A second hash function only

**11. XQuery was developed to:**

- A) Build B+-tree indices
- B) Partition data across shards
- C) Query nested XML structures
- D) Hash search keys into buckets

**12. The CGI standard defines:**

- A) How B+-trees are split
- B) How the web server communicates with application programs
- C) How browsers render HTML
- D) How cookies are encrypted

**13. JavaScript can modify the displayed web page by altering the:**

- A) B+-tree root
- B) HDFS NameNode
- C) DOM (Document Object Model) tree
- D) web.xml file

**14. To create a servlet, the programmer writes a class that inherits from:**

- A) CGIScript
- B) JSPPage
- C) HttpServlet
- D) HttpSession

**15. A line segment is represented in a database by:**

- A) A single vertex
- B) The coordinates of its endpoints
- C) A bitmap of pixels
- D) A hash of its length

**16. In MapReduce, the first attribute of emit() is called the:**

- A) Primary key
- B) Shard key
- C) Search key
- D) Reduce key

**17. A benefit of the LSM approach is that inserts are done using:**

- A) Only sequential I/O operations
- B) Only random I/O operations
- C) Bitmap intersections
- D) No I/O at all

**18. An LSM tree consists of:**

- A) A single hash table
- B) A chain of overflow buckets
- C) Several B+-trees: an in-memory L0 and on-disk L1..Lk
- D) One bitmap per attribute

**19. In a B+-tree, all paths from the root to a leaf are:**

- A) Of the same length
- B) Of different lengths
- C) Equal to the number of attributes
- D) At most 2 nodes long

**20. Which are the two triple forms used by RDF?**

- A) (ID, attribute-name, value) and (ID1, relationship-name, ID2)
- B) (subject, table) and (row, column)
- C) (block, offset) and (key, pointer)
- D) (key, value) and (offset, value)

---

## Answer Key

1-B  2-D  3-C  4-C  5-A  6-B  7-C  8-A  9-D  10-A  11-C  12-B  13-C  14-C  15-B  16-D  17-A  18-C  19-A  20-A
