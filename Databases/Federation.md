A **federation** is a virtual union of several autonomous databases. An intermediate level (federation layer) is created above them, which provides a single interface for executing queries. The data remains in the original databases, each retaining its own schema and engine; the federation only coordinates queries and collects results.

**Advantages**
- **Transparency** - user does not know where the data is physically stored.
- **Possibility to store data in the most suitable for them database** (SQL/NoSQL) and maintaining the autonomy of each source.
- **Possibility to combine data from different systems** (for example, catalogs of different branches) without physically merging databases.

Federation is used to integrate heterogeneous sources (for example, data from different departments, historical databases), when a single point of access and the ability to perform complex queries through middleware are required.