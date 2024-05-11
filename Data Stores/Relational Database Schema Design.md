## Goals
1. **Reduce Redundancy**: divide information carefully among tables to eliminate data redundancy. Duplicate data wastes space and can lead to inconsistency.
2. **Provides access** with information a user needs to join tables together. (Use good primary keys, while creating proper relationships between tables.)
3. Ensures **data accuracy** and **integrity**.
4. Accommodates your data processing and reporting needs.

## Design Process
1. **Define the purpose of the database**: You can help determine what kind of information you need to record into the database by ==sampling queries and what results you want from the query==. From here, gather the information you need, divide them into individual tables. Determine what the columns (fields) of each table will be. Remember, do not repeat information.
2. **Figure out the Primary Key for each table**.
3. **Determine the relationships among the tables**
4. **Refine the Schema**: Questions to ask are:
	1. Do you have enough columns to represent the data ?
	2. Could you derive a column from a combination of columns?
	3. Are you entering duplicate information?
	4. Do you have any empty fields in individual records?
	5. Can a large table be split into two?
	 But to achieve a refined design, you must **normalize** your tables.
## References
1.  https://medium.com/@kimtnguyen/relational-database-schema-design-overview-70e447ff66f9
2. 