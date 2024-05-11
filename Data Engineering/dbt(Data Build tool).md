DBT helps data analysts and engineers to transform data in their warehouses more effectively. 


## dbt and the Modern Data Stack


![[Pasted image 20240506161838.png]]
**dbt is the T in ELT.** It doesn't extract or load data, but it's extremely good at transforming data that's already loaded into your warehouse. This "transform after load" architecture is becoming known as ELT (extract, load, transform).

ELT has become commonplace because of the power of modern analytic databases. Data warehouses like Redshift, Snowflake, and BigQuery are extremely performant _and_ very scalable such that at this point most data transformation use cases can be much more **effectively handled in-database rather than in some external processing layer.** 

Add to this the separation of compute and storage and there are decreasingly few reasons to want to execute your data transformation jobs elsewhere.

dbt is a tool to help you write and execute the data transformation jobs that run inside your warehouse. **dbt's only function is to take code, compile it to SQL, and then run against your database.**









## References
1. https://www.getdbt.com/blog/what-exactly-is-dbt
2. https://github.com/dbt-labs/dbt-core 
3. 