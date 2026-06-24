<!-- BEGIN_TF_DOCS -->
# tf-module-confluent-fonctionnel
> Module Terraform "chapeau" pour la création et la gestion des ressources fonctionnelles (topics) pour les applications métier sur la plateforme Confluent Cloud.

# Description

Ce module Terraform agit comme un module "chapeau" (umbrella), offrant une interface unifiée pour que les équipes applicatives puissent déclarer et gérer leurs ressources sur Confluent Cloud.

Il permet de provisionner de manière déclarative :
-   Des **Topics Kafka**, en incluant leur configuration détaillée (partitions, rétention, etc.) et la gestion de leur schéma associé dans le Schema Registry.dsqsqdsdqsdqsdqdsq

L'objectif est de simplifier la gestion des ressources Kafka pour les applications en centralisant leur définition dans une structure de variables claire.

## Exemples

```hcl
module "confluent_fonctionnel" {
  source = "github.com/ugieiris/tf-module-confluent-fonctionnel?ref=v1.0.0"

  confluent_environment_name = "prep-europe-west9"
  confluent_cluster_name     = "preprod-europe-west9"

  confluent_cloud_cluster_key    = var.my-preprod-api-key
  confluent_cloud_cluster_secret = var.my-preprod-api-secret

  confluent_cloud_schema_registry_key    = var.my-preprod-schema-registry-key
  confluent_cloud_schema_registry_secret = var.my-preprod-schema-registry-secret

  topics = [
    #Topic sans schéma
    {
      name           = "ech.test.testtop003.my-topic-without-schema"
      exchange_topic = false
      config = {
        partitions_count       = 6
        retention_ms           = 604800000 // 7 jours
        cleanup_policy         = "delete"
        max_message_bytes      = 1000
        message_timestamp_type = "CreateTime"
      }
    },
    #Topic avec un schema de type AVRO
    {
      name           = "ech.test.testtop003.my-topic-schema-avro"
      exchange_topic = true
      config = {
        partitions_count       = "2"
        retention_ms           = 604800000
        message_timestamp_type = "LogAppendTime"
        cleanup_policy         = "compact"
        max_message_bytes      = "1000"
      }
      schema = {
        file_path = "./schemas/ech.test.testtop003.my-topic-with-schema.avsc"
      }
    },
    #Topic avec un schema de type JSON
    {
      name           = "ech.test.testtop003.my-topic-with-schema-json"
      exchange_topic = true
      config = {
        partitions_count       = "2"
        retention_ms           = 604800000
        message_timestamp_type = "LogAppendTime"
        cleanup_policy         = "compact"
        max_message_bytes      = "1000"
      }
      schema = {
        file_path = "./schemas/ech.test.testtop003.my-topic-with-schema.json"
        format    = "JSON"
      }
    }
  ]
}

module "confluent_fonctionnel" {
  source = "github.com/ugieiris/tf-module-confluent-fonctionnel?ref=v1.0.0"

  confluent_environment_name = "prod-europe-west9"
  confluent_cluster_name     = "prod-europe-west9"

  confluent_cloud_cluster_key    = var.my-prod-api-key
  confluent_cloud_cluster_secret = var.my-prod-api-secret

  confluent_cloud_schema_registry_key    = var.my-prod-schema-registry-key
  confluent_cloud_schema_registry_secret = var.my-prod-schema-registry-secret

  topics = [
    #Topic avec les paramères obligatoires
    {
      name           = "ech.test.testtop003.my-topic-without-schema"
      exchange_topic = false
      config = {
        partitions_count       = 6
        retention_ms           = 604800000 // 7 jours
        cleanup_policy         = "delete"
        max_message_bytes      = 1000
        message_timestamp_type = "CreateTime"
      },
    },
    #Topic avec un schema de type AVRO
    {
      name           = "ech.test.testtop003.my-topic-schema-avro"
      exchange_topic = true
      config = {
        partitions_count       = "2"
        retention_ms           = 604800000
        message_timestamp_type = "LogAppendTime"
        cleanup_policy         = "compact"
        max_message_bytes      = "1000"
      }
      schema = {
        file_path = "./schemas/ech.test.testtop003.my-topic-with-schema.avsc"
      }
    },
    #Topic avec un schema de type JSON
    {
      name           = "ech.test.testtop003.my-topic-with-schema-json"
      exchange_topic = true
      config = {
        partitions_count       = "2"
        retention_ms           = 604800000
        message_timestamp_type = "LogAppendTime"
        cleanup_policy         = "compact"
        max_message_bytes      = "1000"
      }
      schema = {
        file_path = "./schemas/ech.test.testtop003.my-topic-with-schema.json"
        format    = "JSON"
      }
    }
  ]
}
```

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.10.5 |
| <a name="requirement_confluent"></a> [confluent](#requirement\_confluent) | ~> 2.17.0 |



## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_confluent_topic"></a> [confluent\_topic](#module\_confluent\_topic) | git::https://github.com/ugieiris/tf-module-confluent-kafka-topic | 1.0.0 |



## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_confluent_cloud_cluster_key"></a> [confluent\_cloud\_cluster\_key](#input\_confluent\_cloud\_cluster\_key) | ID de l'api key du cluster confluent | `string` | n/a | yes |
| <a name="input_confluent_cloud_cluster_secret"></a> [confluent\_cloud\_cluster\_secret](#input\_confluent\_cloud\_cluster\_secret) | Secret de l'api key du cluster confluent | `string` | n/a | yes |
| <a name="input_confluent_cloud_schema_registry_key"></a> [confluent\_cloud\_schema\_registry\_key](#input\_confluent\_cloud\_schema\_registry\_key) | ID de l'api key de la schema registry | `string` | n/a | yes |
| <a name="input_confluent_cloud_schema_registry_secret"></a> [confluent\_cloud\_schema\_registry\_secret](#input\_confluent\_cloud\_schema\_registry\_secret) | Secret de l'api key de la schema registry | `string` | n/a | yes |
| <a name="input_confluent_cluster_name"></a> [confluent\_cluster\_name](#input\_confluent\_cluster\_name) | Nom du cluster Confluent.<br/>    Correspond à la première clé du champ clusters de l'environnement choisi https://github.com/ugieiris/tf-module-references-pfei/blob/master/README.md | `string` | n/a | yes |
| <a name="input_confluent_environment_name"></a> [confluent\_environment\_name](#input\_confluent\_environment\_name) | Nom de l'environnement Confluent.<br/>    Correspond à la première clé de la map confluent\_environments https://github.com/ugieiris/tf-module-references-pfei/blob/master/README.md | `string` | n/a | yes |
| <a name="input_topics"></a> [topics](#input\_topics) | n/a | <pre>list(object({<br/>    name           = string<br/>    exchange_topic = optional(bool, false)<br/>    config = object({<br/>      partitions_count                    = number<br/>      retention_ms                        = optional(number, null)<br/>      message_timestamp_type              = string<br/>      cleanup_policy                      = string<br/>      max_message_bytes                   = number<br/>      delete_retention_ms                 = optional(number, 86400000)<br/>      max_compaction_lag_ms               = optional(number, 9223372036854775807)<br/>      min_compaction_lag_ms               = optional(number, 0)<br/>      message_timestamp_difference_max_ms = optional(number, 9223372036854775807)<br/>      message_timestamp_before_max_ms     = optional(number, 9223372036854775807)<br/>      message_timestamp_after_max_ms      = optional(number, 9223372036854775807)<br/>      min_insync_replicas                 = optional(number, 2)<br/>      retention_bytes                     = optional(number, -1)<br/>      segment_bytes                       = optional(number, 104857600)<br/>      segment_ms                          = optional(number, 604800000)<br/>    })<br/>    schema = optional(object({<br/>      format              = optional(string, "AVRO")<br/>      file_path           = optional(string, "")<br/>      compatibility_level = string<br/>      metadata_properties = optional(map(string), {})<br/>    }), null)<br/>    metadata = optional(object({<br/>      domain      = string<br/>      owner       = string<br/>      contactInfo = string<br/>    }), null)<br/>  }))</pre> | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_confluent_schemas"></a> [confluent\_schemas](#output\_confluent\_schemas) | La ressource complète du schéma Confluent créé par le module enfant. |
| <a name="output_confluent_topics"></a> [confluent\_topics](#output\_confluent\_topics) | La ressource complète du topic Kafka créé par le module enfant. |

# Liens externes

- Configuration d'un topic Kafka: [Doc Confluent](https://docs.confluent.io/platform/current/installation/configuration/topic-configs.html)
<!-- END_TF_DOCS -->
