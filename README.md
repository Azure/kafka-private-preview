# Kafka Input and Output Private Preview

You can connect an Azure Stream Analytics job directly to Kafka clusters to ingest natively and output data. The solution is low code and entirely managed by the Azure Stream Analytics team at Microsoft, allowing it to meet business compliance standards. The Kafka Adapters are backward compatible and support all Kafka versions with the latest client release. Users can connect to Kafka clusters inside a VNET and Kafka clusters with a public endpoint depending on the configurations. The configuration relies on existing Kafka configuration conventions. 


## Authentication and Encryption 

Azure Stream Analytics supports four types of security protocols to connect to your Kafka setup: 

1. mTLS or SSL/TLS – encryption and authentication (recommended). 
2. SASL_SSL – It combines two different security mechanisms - SASL (Simple Authentication and Security Layer) and SSL (Secure Sockets Layer) - to ensure both authentication and encryption are in place for data transmission. 
3. SASL_PLAINTEXT – standard authentication with username and password. 
4. None – No authentication or encryption. Recommended only for testing. 

## Key Vault Integration 

Note: To use mTLS or SASL_SSL security protocols, you must have Azure KeyVault and managed identity configured for your Azure Stream Analytics job. 

Azure Stream Analytics integrates seamlessly with Azure Key Vault to access stored secrets needed for authentication and encryption when using mTLS or SASL_SSL security protocols. Your Azure Stream Analytics job connects to Azure Key Vault using managed identity to ensure secure connection and avoid exfiltration of secrets. 

You can store the certificates as Key Vault certificate or Key Vault secret. Private keys are in PEM format. 
Please visit the following to learn how to upload secrets to Azure Key Vault: [Store a multiline secret in Azure Key Vault](https://learn.microsoft.com/azure/key-vault/secrets/multiline-secrets)


## VNET Configuration 

When configuring your Azure Stream Analytics job to connect to your Kafka clusters, depending on your configuration, you may have to configure your job to be able to access your Kafka clusters which are behind a firewall or inside a virtual network.  Please visit the ASA VNET documentation to learn more about configuring private endpoints to access resources which are inside a virtual network or behind a firewall. 

## Configuration 

The following table lists the property names and their description for creating a Kafka Input or Output: 
| Property name                | Description                                                                                                             |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| Input/Output Alias            | A friendly name used in queries to reference your input or output                                                       |
| Bootstrap server addresses   | A list of host/port pairs to use for establishing the connection to the Kafka cluster.                                  |
| Kafka topic                  | A unit of your Kafka cluster you want to write events to.                                                               |
| Security Protocol            | How you want to connect to your Kafka cluster. Azure Stream Analytics supports: mTLS, SASL_SSL, SASL_PLAINTEXT or None. |
| Event Serialization format   | The serialization format (JSON, CSV, Avro, Parquet) of the incoming data stream.                                        |
| Partition key                | Azure Stream Analytics assigns partitions using round partitioning.                                                     |
| Kafka event compression type | The compression type used for outgoing data stream, such as Gzip, Snappy, Lz4, Zstd or None.                            |

## Limitations

* When configuring your Azure Stream Analytics jobs to use VNET/SWIFT, your job must be configured with a minimum of six (6) streaming units. 
* When using mTLS or SASL_SSL with Azure Key Vault, you must convert your Java Key Store to PEM format. 
* Sample data is not supported currently for all modes except None and SASL_PLAINTEXT 
* The minimum version of Kafka you can configure Azure Stream Analytics to connect to is version 0.10. 

