# ETL (Extract, Transform, Load)

#### ETL is a process that extracts, transforms, and loads data from multiple sources to a data warehouse or other unified data repository.

## Table of Contents

- [What is ETL?](https://www.ibm.com/cloud/learn/etl#toc-what-is-et-xeCDpL69)
- [How ETL works](https://www.ibm.com/cloud/learn/etl#toc-how-etl-wo-VM4S8Yrn)
- [ETL vs ELT](https://www.ibm.com/cloud/learn/etl#toc-etl-vs-elt-goFgkQcP)
- [ETL and other data integration methods](https://www.ibm.com/cloud/learn/etl#toc-etl-and-ot-NiTFjp1v)
- [ETL tools](https://www.ibm.com/cloud/learn/etl#toc-etl-tools-BuD3ej6B)
- [ETL, data integration, and IBM Cloud](https://www.ibm.com/cloud/learn/etl#toc-etl-data-i-vtLPdWke)

## What is ETL?

*ETL*, for *extract, transform and load**,* is a data integration process that combines data from multiple data sources into a single, consistent data store that is loaded into a [data warehouse](https://www.ibm.com/cloud/learn/data-warehouse) or other target system.

ETL was introduced in the 1970s as a process for integrating and loading data into mainframes or supercomputers for computation and analysis. From the late 1980s through the mid 2000s, it was the primary process for creating data warehouses that support business intelligence (BI) applications. A time-consuming batch operation, ETL is now recommended more often for creating smaller target data repositories that require less-frequent updating, while other data integration methods—including ELT (extract, load, transform), CDC, and data virtualization—are used to integrate increasingly larger volumes of constantly-changing data or real-time data streams.

[Learn more about data integration](https://www.ibm.com/analytics/data-integration).

## How ETL works

The easiest way to understand how ETL works is to understand what happens in each step of the process.

### Extract

In the data extraction step, data is copied or exported from source locations to a staging area. The data can come from virtually any structured or unstructured source—SQL or [NoSQL](https://www.ibm.com/cloud/learn/nosql-databases) servers, CRM and ERP systems, text and document files, emails, web pages, and more.

### Transform

In the staging area, the raw data is transformed to be useful for analysis and to fit the schema of the eventual target data warehouse, which is typically powered by a structured online analytical processing (OLAP) or [relational database](https://www.ibm.com/cloud/learn/relational-databases). This can involve the following:

- Filtering, cleansing, de-duplicating, validating, and authenticating the data.
- Performing calculations, translations, or summaries based on the raw data. This can include everything from changing row and column headers for consistency, to converting currencies or units of measurement, to editing text strings, to summing or averaging values—whatever is needed to suit the organization’s specific BI or analytical purposes.
- Removing, encrypting, hiding, or otherwise protecting data governed by government or industry regulations.
- Formatting the data into tables or joined tables to match the schema of the target data warehouse

Performing these transformations in a staging area—as opposed to within the source systems themselves—limits the performance impact on the source systems and reduces the likelihood of data corruption.

### Load

In this last step, the transformed data is moved from the staging area into a target data warehouse. Typically, this involves an initial loading of all data, followed by periodic loading of incremental data changes and, less often, full refreshes to erase and replace data in the warehouse.

For most organizations that use ETL, the process is automated, well-defined, continuous, and batch-driven—run during off-hours when traffic on the source systems and the data warehouse is at its lowest.

## ETL vs ELT

*ELT* *(\*extract, load, transform)**—reverses the second and third steps of the ETL process. It copies or exports the data from the source locations, but instead of moving it to a staging area for transformation, it loads the raw data directly to the target data store, where it can be transformed as needed.

The order of steps is not the only difference. In ELT, the target data store can be a data warehouse, but more often it is a *data lake*, which is a large central store designed to hold both structured and unstructured data at massive scale. Data lakes are managed using a big data platform (such as Apache Hadoop) or a distributed NoSQL data management system. They can support business intelligence, but more often, they’re created to support artificial intelligence, machine learning, predictive analytics and applications driven by real-time data and event streams.

The following video explains more about data lakes:

[Watch the video](https://www.ibm.com/cloud/learn/etl)

There are other differences between ETL and ELT. For example, because it transforms data before moving it to the central repository, ETL can make data privacy compliance simpler, or more systematic, than ELT (e.g., if analysts don’t transform sensitive data before they need to use it, it could sit unmasked in the data lake).

Data scientists might prefer ELT, which lets them play in a ‘sandbox’ of raw data and do their own data transformation tailored to specific applications. But, in most cases, the choice between ETL and ELT will depend on the choice between data warehouse or data lake.

## ETL and other data integration methods

ETL and ELT are just two tools in the data integration toolbox. Other data integration methods that can be used with or instead of ETL or ELT include the following:

- **Change data capture (CDC)** identifies and captures only the source data that has changed and moves that data to the target system. CDC can be used to reduce the resources required during the ETL ‘extract’ step; it can also be used on its own to move changed data directly to a data lake or other repository in real time.
- **Data replication** copies changes in data sources in real time or in batches to a central database. Data replication is often listed as a data integration method but, in fact, is most often used to create backups for [disaster recovery](https://www.ibm.com/cloud/learn/disaster-recovery-introduction).
- **Data virtualization** uses a software abstraction layer to create a unified, integrated, fully usable *view* of data—without physically copying, transforming, or loading the source data to a target system. Data virtualization enables an organization to create virtual data warehouses, data lakes, and data marts from the same source data, without the expense and complexity of building and managing separate platforms for each. While data virtualization can be used alongside ETL, it is increasingly seen as an alternative to ETL and to other physical data integration methods.
- **Stream data integration (SDI)** is just what it sounds like—it continuously consumes data streams in real time, transforms them, and loads them to a target system for analysis. The key word here is *continuously*; instead of integrating snapshots of data extracted from sources at a given time, it integrates data constantly as it becomes available. Stream data integration enables a data store for powering analytics, machine learning, and real-time applications for improving customer experience, fraud detection, and more.  

According to the [2019 Gartner Magic Quadrant for Data Integration Tools](https://www.ibm.com/account/reg/signup?formid=urx-34814), by 2021, more than 80% of organizations will use more than one of these methods to execute their data integration use cases.

## ETL tools

Once upon a time, organizations wrote their own ETL code, but there are now many open source and commercial ETL tools and cloud services to choose from. Typical benefits of these products include the following:

- **Comprehensive automation and ease of use:** Leading ETL tools automate the entire data flow, from data sources to the target data warehouse; many tools recommend rules for extracting, transforming, and loading the data.
- **A visual, drag-and-drop interface:** This can be used for specifying rules and data flows.
- **Support for complex data management:** This includes assistance with complex calculations, data integrations, and string manipulations.
- **Security and compliance:** The best ETL tools encrypt data both in motion and at rest and are certified compliant with industry or government regulations like HIPAA and GDPR.

In addition, many ETL tools have evolved to include ELT capability and to support [integration of real-time and streaming data for artifical intelligence (AI) applications](https://www.ibm.com/products/infosphere-datastage).