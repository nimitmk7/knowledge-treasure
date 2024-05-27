
## ELT
![[Pasted image 20240506161919.png]]
**Extract**, **Load**, **Transform** (ELT) is the process of first extracting data from different data sources, then loading it into a target data warehouse, and finally transforming it.

ELT has emerged as a paradigm for how to manage information flows in a modern data warehouse. This represents a fundamental shift from how data previously was handled when **Extract**, **Transform**, **Load** (ETL) was the data workflow most companies implemented.

Transitioning from ETL to ELT means that you no longer have to capture your transformations during the initial loading of the data into your data warehouse. Rather, you are able to load all of your data, then build transformations on top of it.
## Extract

In the extraction process, data is extracted from multiple data sources. The data extracted is, for the most part, data that teams eventually want to use for analytics work. Some examples of data sources can include:

- Backend application databases(MySQL, Postgres, etc.)
- Marketing platforms(Google Ads, Marketo, etc.)
- Email and sales CRMs.(Salesforce, Hubspot, Netsuite, etc.)

Depending on source, extraction is done in either of 3 ways:
1. API calls
2. DB queries
3. Receive data on web-hook

In the extraction phase, you need to validate your data and reject the data that doesn’t meet your requirements. For rejected data, you might have to notify the sources. 
## Load
During the loading stage, data that was extracted is loaded into the target data warehouse. 

Some examples of modern data warehouses include Snowflake, Amazon Redshift, and Google BigQuery. Examples of other data storage platforms include data lakes such as Databricks’s Data Lakes. 

At this point in the ELT process, the data is mostly unchanged from its point of extraction. If you use an extraction and loading tool like Fivetran or Hevo Data, there may have been some light normalization on your data. But for all intents and purposes, the data loaded into your data warehouse at this stage is in its raw format.

## Transform
In the final transformation step, the raw data that has been loaded into your data warehouse is finally ready for modeling! When you first look at this data, you may notice a few things about it…

- Column names may or may not be clear
- Some columns are potentially the incorrect data type
- Tables are not joined to other tables
- Timestamps may be in the incorrect timezone for your reporting
- JSON fields may need to be unnested
- Tables may be missing primary keys.
- And more!

...hence the need for transformation! During the transformation process, data from your data sources is usually:

- **Lightly Transformed**: Fields are cast correctly, timestamp fields’ timezones are made uniform, tables and fields are renamed appropriately, and more.
- **Heavily Transformed**: Business logic is added, appropriate materializations are established, data is joined together, etc.
- **QA’d**: Data is tested according to business standards. In this step, data teams may ensure primary keys are unique, model relations match-up, column values are appropriate, and more.

Common ways to transform your data include leveraging modern technologies such as dbt, writing custom SQL scripts that are automated by a scheduler, utilizing stored procedures, and more.

## Benefits
### ELT benefit #1: Data as code
#### Analytics code can now follow the same best practices as software code[​](https://docs.getdbt.com/terms/elt#analytics-code-can-now-follow-the-same-best-practices-as-software-code "Direct link to Analytics code can now follow the same best practices as software code")

At its core, data transformations that occur last in a data pipeline allow for code-based and version-controlled transformations. These two factors alone permit data team members to:

- Easily recreate historical transformations by rolling back commits
- Establish code-based tests
- Implement CI/CD workflows
- Document data models like typical software code.
#### Scaling, made sustainable[​](https://docs.getdbt.com/terms/elt#scaling-made-sustainable "Direct link to Scaling, made sustainable")

As your business grows, the number of data sources correspondingly increases along with it. As such, so do the number of transformations and models needed for your business. Managing a high number of transformations without version control or automation is not scalable.

The ELT workflow capitalizes on transformations occurring last to provide flexibility and software engineering best practices to data transformation. Instead of having to worry about how your extraction scripts scale as your data increases, data can be extracted and loaded automatically with a few clicks.

### ELT benefit #2: Bring the power to the people[​](https://docs.getdbt.com/terms/elt#elt-benefit-2-bring-the-power-to-the-people "Direct link to ELT benefit #2: Bring the power to the people")

The ELT workflow opens up a world of opportunity for the people that work on that data, not just the data itself.

#### Empowers data team members[​](https://docs.getdbt.com/terms/elt#empowers-data-team-members "Direct link to Empowers data team members")

Data analysts, analytics engineers, and even data scientists no longer have to be dependent on data engineers to create custom pipelines and models. Instead, they can use point-and-click products such as Fivetran and Airbyte to extract and load the data for them.

Having the transformation as the final step in the ELT workflow also allows data folks to leverage their understanding of the data and SQL to focus more on actually modeling the data.

#### Promotes greater transparency for end busines users[​](https://docs.getdbt.com/terms/elt#promotes-greater-transparency-for-end-busines-users "Direct link to Promotes greater transparency for end busines users")

Data teams can expose the version-controlled code used to transform data for analytics to end business users by no longer having transformations hidden in the ETL process. Instead of having to manually respond to the common question, “How is this data generated?” data folks can direct business users to documentation and repositories. Having end business users involved or viewing the data transformations promote greater collaboration and awareness between business and data folks.

## References

1. https://docs.getdbt.com/terms/elt
2. 