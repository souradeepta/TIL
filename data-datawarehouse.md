# Data Warehouse

#### A core component of business intelligence, a data warehouse pulls together data from many different sources into a single data repository for sophisticated analytics and decision support.

## Table of Contents

- [What is a data warehouse?](https://www.ibm.com/cloud/learn/data-warehouse#toc-what-is-a--1NvW7Mkj)
- [Data warehouse architecture](https://www.ibm.com/cloud/learn/data-warehouse#toc-data-wareh-n_j6aLQQ)
- [Understanding OLAP and OLTP in data warehouses](https://www.ibm.com/cloud/learn/data-warehouse#toc-understand-U63sa2WD)
- [Schemas in data warehouses](https://www.ibm.com/cloud/learn/data-warehouse#toc-schemas-in-G772j9ny)
- [Data warehouse vs. database, data lake, and data mart](https://www.ibm.com/cloud/learn/data-warehouse#toc-data-wareh-86wFunzB)
- [Types of Data Warehouses](https://www.ibm.com/cloud/learn/data-warehouse#toc-types-ofda-z4wJKT4X)
- [Benefits of a data warehouse](https://www.ibm.com/cloud/learn/data-warehouse#toc-benefits-o-K6Q2ssNI)
- [Data warehouse and IBM cloud](https://www.ibm.com/cloud/learn/data-warehouse#toc-data-wareh-2x6sXhK6)

## What is a data warehouse?

A data warehouse, or enterprise data warehouse (EDW), is a system that aggregates data from different sources into a single, central, consistent data store to support data analysis, data mining, artificial intelligence (AI), and machine learning. A data warehouse system enables an organization to run powerful analytics on huge volumes (petabytes and petabytes) of historical data in ways that a standard database cannot.

Data warehousing systems have been a part of business intelligence (BI) solutions for over three decades, but they have evolved recently with the emergence of new data types and data hosting methods. Traditionally, a data warehouse was hosted on-premises—often on a mainframe computer—and its functionality was focused on extracting data from other sources, cleansing and preparing the data, and loading and maintaining the data in a relational database. More recently, a data warehouse might be hosted on a dedicated appliance or in the cloud, and most data warehouses have added analytics capabilities and data visualization and presentation tools.

[*Find out more about data warehouse solutions from IBM.*](https://www.ibm.com/analytics/data-warehouse)

## Data warehouse architecture

Generally speaking, data warehouses have a three-tier architecture, which consists of a:

- **Bottom tier:** The bottom tier consists of a data warehouse server, usually a relational database system, which collects, cleanses, and transforms data from multiple data sources through a process known as Extract, Transform, and Load (ETL) or a process known as Extract, Load, and Transform (ELT).
- **Middle tier:** The middle tier consists of an OLAP (i.e. online analytical processing) server which enables fast query speeds. Three types of OLAP models can be used in this tier, which are known as ROLAP, MOLAP and HOLAP. The type of OLAP model used is dependent on the type of database system that exists.
- **Top tier:** The top tier is represented by some kind of front-end user interface or reporting tool, which enables end users to conduct ad-hoc data analysis on their business data.

## Understanding OLAP and OLTP in data warehouses

OLAP (for *online analytical processing*) is software for performing multidimensional analysis at high speeds on large volumes of data from unified, centralized data store, like a data warehouse. OLTP, or online transactional processing, enables the real-time execution of large numbers of database transactions by large numbers of people, typically over the internet. The main difference between OLAP and OLTP is in the name: OLAP is analytical in nature, and OLTP is transactional. 

OLAP tools are designed for multidimensional analysis of data in a data warehouse, which contains both historical and transactional data. Common uses of OLAP include data mining and other business intelligence applications, complex analytical calculations, and predictive scenarios, as well as business reporting functions like financial analysis, budgeting, and forecast planning.

OLTP is designed to support transaction-oriented applications by processing recent transactions as quickly and accurately as possible. Common uses of OLTP include ATMs, e-commerce software, credit card payment processing, online bookings, reservation systems, and record-keeping tools.

## Schemas in data warehouses

Schemas are ways in which data is organized within a database or data warehouse. There are two main types of schema structures, the star schema and the snowflake schema, which will impact the design of your data model.

**Star schema:** This schema consists of one fact table which can be joined to a number of denormalized dimension tables. It is considered the simplest and most common type of schema, and its users benefit from its faster speeds while querying.

**Snowflake schema:** While not as widely adopted, the snowflake schema is another organization structure in data warehouses. In this case, the fact table is connected to a number of normalized dimension tables, and these dimension tables have child tables. Users of a snowflake schema benefit from its low levels of data redundancy, but it comes at a cost to query performance. 

## Data warehouse vs. database, data lake, and data mart

Data warehouse, database, data lake, and data mart are all terms that tend to be used interchangeably. While the terms are similar, important differences exist:

### Data warehouse vs. data lake

A data warehouse gathers raw data from multiple sources into a central repository, structured using predefined schemas designed for data analytics. A data lake is a data warehouse without the predefined schemas. As a result, it enables more types of analytics than a data warehouse. Data lakes are commonly built on big data platforms such as Apache Hadoop.

See the following video for more information on data lakes:

<iframe width="854" height="480" frameborder="0" allowfullscreen="allowfullscreen" mozallowfullscreen="mozAllowFullscreen" webkitallowfullscreen="webkitallowfullscreen" allow="autoplay *; fullscreen *; encrypted-media *" src="https://cdnapisec.kaltura.com/html5/html5lib/v2.76/mwEmbedFrame.php/p/1773841/uiconf_id/39954662/entry_id/1_0nq8n3v3?wid=_1773841&amp;iframeembed=true&amp;playerId=kplayer&amp;entry_id=1_0nq8n3v3&amp;flashvars[streamerType]=auto" style="box-sizing: inherit; margin: 0px; padding: 0px; border: 0px; vertical-align: baseline; font-size: inherit; font-family: &quot;IBM Plex Sans&quot;, ibm-plex-sans, &quot;Helvetica Neue&quot;, Arial, sans-serif; position: absolute; top: 0px; left: 0px; width: 727.6px; height: 409.275px;"></iframe>

### Data warehouse vs. data mart

A data mart is a subset of a data warehouse that contains data specific to a particular business line or department. Because they contain a smaller subset of data, data marts enable a department or business line to discover more-focused insights more quickly than possible when working with the broader data warehouse data set.

### Data warehouse vs. database

A database is built primarily for fast queries and transaction processing, not analytics. A database typically serves as the focused data store for a specific application, whereas a data warehouse stores data from any number (or even all) of the applications in your organization.

A database focuses on updating real-time data while a data warehouse has a broader scope, capturing current and historical data for predictive analytics, machine learning, and other advanced types of analysis.

## Types of Data Warehouses

### Cloud data warehouse

A cloud data warehouse is a data warehouse specifically built to run in the cloud, and it is offered to customers as a managed service. Cloud-based data warehouses have grown more popular over the last five to seven years as more companies use cloud services and seek to reduce their on-premises [data center](https://www.ibm.com/cloud/learn/data-centers) footprint.

With a cloud data warehouse, the physical data warehouse infrastructure is managed by the cloud company, meaning that the customer doesn’t have to make an upfront investment in hardware or software and doesn’t have to manage or maintain the data warehouse solution.

### Data warehouse software (on-premises/license)

A business can purchase a data warehouse license and then deploy a data warehouse on their own on-premises infrastructure. Although this is typically more expensive than a cloud data warehouse service, it might be a better choice for government entities, financial institutions, or other organizations that want more control over their data or need to comply with strict security or data privacy standards or regulations.

### Data warehouse appliance

A data warehouse appliance is a pre-integrated bundle of hardware and software—CPUs, storage, operating system, and data warehouse software—that a business can connect to its [network](https://www.ibm.com/cloud/learn/networking-a-complete-guide) and start using as-is. A data warehouse appliance sits somewhere between cloud and on-premises implementations in terms of upfront cost, speed of deployment, ease of scalability, and management control.

## Benefits of a data warehouse

A data warehouse provides a foundation for the following:

- **Better data quality:** A data warehouse centralizes data from a variety of data sources, such as transactional systems, operational databases, and flat files. It then cleanses it, eliminates duplicates, and standardizes it to create a single source of the truth.
- **Faster, business insights:** Data from disparate sources limit the ability of decision makers to set business strategies with confidence. Data warehouses enable data integration, allowing business users to leverage all of a company’s data into each business decision.
- **Smarter decision-making:**  A data warehouse supports large-scale BI functions such as data mining (finding unseen patterns and relationships in data), artificial intelligence, and machine learning—tools data professionals and business leaders can use to get hard evidence for making smarter decisions in virtually every area of the organization, from business processes to financial management and inventory management
- **Gaining and growing competitive advantage:** All of the above combine to help an organization finding more opportunities in data, more quickly than is possible from disparate data stores.

## Data warehouse and IBM cloud

IBM offers on-premises, cloud, and integrated appliance [data warehouse solutions](https://www.ibm.com/analytics/data-warehouse)—all built on a data analytics and artificial intelligence foundation optimized for predictive insight and data-driven decision making. All three are part of the IBM DB2 family of products, offering a common SQL engine to streamline queries and machine learning capabilities that enhance data management performance.

[IBM Db2 Warehouse on Cloud](https://www.ibm.com/cloud/db2-warehouse-on-cloud) is a fully managed, elastic cloud data warehouse that delivers independent scaling of storage and compute, featuring a highly optimized columnar data store, actionable compression, and in-memory processing to supercharge your analytics and machine learning workloads. Smaller data marts and spin ups can add [Flex One](https://www.ibm.com/cloud/blog/db2-warehouse-flex-one), an elastic data warehouse built for high-performance analytics, deployable on multiple cloud providers, starting at 40 GB of storage.

[Netezza Performance Server](https://www.ibm.com/analytics/netezza), the next evolution of the IBM Netezza appliance, builds on the hyper-converged architecture of the [IBM Cloud Pak for Data System](https://www.ibm.com/products/cloud-pak-for-data/system) to provide a [cloud native](https://www.ibm.com/cloud/learn/cloud-native) decision support system for your enterprise’s most complex analytics.

[IBM InfoSphere DataStage](https://www.ibm.com/products/infosphere-datastage?mhsrc=ibmsearch_a&mhq=InfoSphere DataStage ), a data warehouse tool that delivers advanced enterprise ETL and provides a [multicloud](https://www.ibm.com/cloud/learn/multicloud) platform that integrates data across multiple enterprise systems.

And [IBM Watson Studio](https://www.ibm.com/cloud/watson-studio), a data science and machine-learning offering, empowers organizations to tap into data assets and inject predictions into business processes and modern applications.