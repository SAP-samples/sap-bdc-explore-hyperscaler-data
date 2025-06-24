# Explore Hyperscaler data with SAP Business Data Cloud
<!--- Register repository https://api.reuse.software/register, then add REUSE badge:
[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/REPO-NAME)](https://api.reuse.software/info/github.com/SAP-samples/REPO-NAME)
-->

## Description
SAP Datasphere, which is an integral part of SAP Business Data Cloud, empowers organizations with a comprehensive business data fabric architecture, seamlessly integrating mission-critical data from across the enterprise. <br> In the Open data ecosystem integration architecture, SAP Datasphere goes beyond merely connecting SAP data; it also integrates non-SAP data from leading hyperscaler services such as AWS, Microsoft Azure, and Google Cloud Platform. This repository offers step-by-step guidance for integrating hyperscaler data sources with SAP Datasphere.

## Requirements
<ul><li>SAP Business Data Cloud's SAP Datasphere instance </li>
	<li>Access details needed for the hyperscaler data source to be integrated (specified in the corresponding READMEs).</li>
</ul>

## Integration Instructions

#### For integrating AWS data sources: ####
 
[Amazon Athena integration](https://github.com/SAP-samples/sap-bdc-explore-hyperscaler-data/AWS/athena-integration.md) <br>
[Amazon Redshift integration](/AWS/redshift-integration.md) <br>
[Amazon S3 integration](/AWS/s3-integration.md)

#### For integrating GCP data sources: ####

[Google BigQuery data federation](GCP/bigquery-data-federation.md)<br>
[For Google BigQuery data replication](GCP/bigquery-data-replication.md)

#### For integrating Azure data sources: ####
[MS Fabric integration](Azure/fabric-integration.md) <br>

#### For integrating Databricks data sources: ####
[For Databricks delta lake federation](databricks/databricks-integration.md)


## Known Issues
No known issues

## How to obtain support
[Create an issue](https://github.com/SAP-samples/sap-bdc-explore-hyperscaler-data/issues) in this repository if you find a bug or have questions about the content.
 
For additional support, [ask a question in SAP Community](https://answers.sap.com/questions/ask.html).

## Contributing
If you wish to contribute code, offer fixes or improvements, please send a pull request. Due to legal reasons, contributors will be asked to accept a DCO when they create the first pull request to this project. This happens in an automated fashion during the submission process. SAP uses [the standard DCO text of the Linux Foundation](https://developercertificate.org/).

## License
Copyright (c) 2024 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSE) file.
