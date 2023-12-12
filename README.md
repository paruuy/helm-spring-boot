# Helm Chart for Spring Boot Application

This Helm chart is designed to deploy a Spring Boot application on Kubernetes. It includes manifests for Ingress, Service, and Deployment, allowing you to easily manage and deploy your Spring Boot application.

## Table of Contents

- [Manifests](#manifests)
  - [Ingress](#ingress)
  - [Service](#service)
  - [Deployment](#deployment)
  - [ConfigMap](#configmap)
  - [ExternalSecret](#externalsecret)
---

## Manifests

### Ingress

The Ingress manifest (`ingress.yaml`) defines the rules for routing external traffic to your Spring Boot application. You can customize the following aspects:

- `ingress.annotations`: Additional annotations for the Ingress resource.
- `ingress.className`: The class to be used for this Ingress.
- `ingress.domainSuffix`: The domain suffix for your application to create the host value using the application.name + domainSuffiz. (This value is harcoded by ArgoCD when using CaaS).
- `ingress.host.enabled`: Enable/disable to add a hostname for your application (For CaaS the host is "{{ .Values.application.name }}.{{ .Values.ingress.domainSuffix }}").
- `ingress.contextPath`: The context path to access to your application.
- `ingress.pathType`: The path type for your Ingress.
- `ingress.tls.enabled`: Enable/disable TLS for your Ingress.
- `ingress.tls.tlsSecretName`: The name of the TLS secret (This value is harcoded by ArgoCD when using CaaS). 

### Service

The Service manifest (`service.yaml`) exposes your Spring Boot application within the Kubernetes cluster. Customize the following:

- `service.port`: The port for your service.
- `service.targetPort`: The target port for your service.

### Deployment

The Deployment manifest (`deployment.yaml`) specifies how your Spring Boot application should be deployed. Customize:

- `application.name`: The name of your Spring Boot application.
- `replicaCount`: The number of replicas for your application.
- `image.registry`: The Docker image registry for your application (This value is harcoded by ArgoCD when using CaaS). The value needs to have the '/' at the end and this field is used together with the 'image.repositoryPath' field and 'image.tag' to create the fully URL of the application's docker image. The format is '{{ .Values.image.registry }}{{ .Values.image.repositoryPath }}:{{ .Values.image.tag }}' .
- `image.repositoryPath`: The 'image.repositoryPath' field is used together with the 'image.registry' field and 'image.tag' to create the fully URL of the application's docker image. The format is '{{ .Values.image.registry }}{{ .Values.image.repositoryPath }}:{{ .Values.image.tag }}'
- `image.tag`: The Docker image tag for your application. (For CaaS that parameter can be used in application.yaml file)
- `image.pullPolicy`: The image pull policy.
- `image.pullSecret`: The secret for pulling the Docker image (This value is harcoded by ArgoCD when using CaaS).
- `debug.enabled`: Enable/disable Java remote debugging.
- `debug.port`: The port for Java remote debugging.
- `probes.enabled`: Enable/disable readiness, liveness, and startup probes.
- `probes.readinessProbe`: Configuration for the readiness probe.
- `probes.livenessProbe`: Configuration for the liveness probe.
- `probes.startupProbe`: Configuration for the startup probe.

### ConfigMap

If Spring Property Config is enabled (springPropConfig.enable set to true), a ConfigMap manifest (`cm-app-properties.yaml`) is provided to create a ConfigMap from the application.properties file to be able to customize your propertie field by environments. 

Customize the following in the ConfigMap YAML:

- `springPropConfig.enable`: Set to true to enable the creation of the config map.
- `springPropConfig.applicationPropertiesPath`: Path of the file content of the application.properties file (The path of the file to be loaded must be located in app-properties folder).

### ExternalSecret

If external secrets are enabled (externalSecrets.enabled set to true), an External Secret manifest (`external-secret.yaml`) is provided to create a secret with sensitive values needed by the application and obtained from a secrets provider (in caas the provider and Hashicorp Vault).

Customize the following in the External Secret YAML:

- `externalSecrets.enabled`: Determines whether External Secrets are enabled (true to enable, false to disable).

- `externalSecrets.secretStoreRefName`: Specifies the name of the secret store reference. To use this helm chart in CaaS the value must be "vault-backend-customer"

- `externalSecrets.secretStoreRefKind`: Defines the type of the secret store reference. Most used kind is "SecretStore" (CaaS).

- `externalSecrets.secretData`: Specifies an array of secret configurations, each containing the following details:

  - `externalSecrets.secretData.secretKey`: The key used in the External Secret to identify the secret data (Key created in the Secret manifest result of this External Secret).

  - `externalSecrets.secretData.secretName`: The name of the External Secret, typically generated using a combination of the application name and "external-secrets"

  - `externalSecrets.secretData.remoteRefKey`: The path name of the secret manager where the fields declared in 'property' are the keys to retrieve the values.

  - `externalSecrets.secretData.property`: The property key name used in the secret manager to store the corresponding value.

  - `externalSecrets.secretData.envVarName`: The name of the environment variable to be created dynamically in the deployment.yaml, allowing the application to use the secret value.

