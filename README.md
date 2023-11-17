# Helm Chart for Spring Boot Application

This Helm chart is designed to deploy a Spring Boot application on Kubernetes. It includes manifests for Ingress, Service, and Deployment, allowing you to easily manage and deploy your Spring Boot application.

## Table of Contents

- [Manifests](#manifests)
  - [Ingress](#ingress)
  - [Service](#service)
  - [Deployment](#deployment)
- [Customizable Values](#customizable-values)

---

## Manifests

### Ingress

The Ingress manifest (`ingress.yaml`) defines the rules for routing external traffic to your Spring Boot application. You can customize the following aspects:

- Annotations: Additional annotations for the Ingress resource.
- Ingress Class: The class to be used for this Ingress.
- Host: The hostname for your application (if enabled).
- Context Path: The context path for your application.
- TLS Configuration: Enable TLS and specify the TLS secret name.

### Service

The Service manifest (`service.yaml`) exposes your Spring Boot application within the Kubernetes cluster. Customize the following:

- Port Configuration: Define the ports for your service.
- Selector: Label selectors for identifying the pods associated with this service.

### Deployment

The Deployment manifest (`deployment.yaml`) specifies how your Spring Boot application should be deployed. Customize:

- Replica Count: The number of replicas for your application.
- Image Configuration: Specify the Docker image, pull policy, and imagePullSecrets if required.
- Environment Variables: Customize environment variables, such as Java options.
- Probes: Configure readiness, liveness, and startup probes.

## Customizable Values

Here is a list of customizable values in the `values.yaml` file and their purposes:

- `application.name`: The name of your Spring Boot application.
- `ingress.annotations`: Additional annotations for the Ingress resource.
- `ingress.className`: The class to be used for this Ingress.
- `ingress.domainSuffix`: The domain suffix for your application to create the host value using the application.name + domainSuffiz.
- `ingress.host.enabled`: Enable/disable hostname for your application.
- `ingress.contextPath`: The context path for your application.
- `ingress.pathType`: The path type for your Ingress.
- `ingress.tls.enabled`: Enable/disable TLS for your Ingress.
- `ingress.tls.tlsSecretName`: The name of the TLS secret. 
- `service.port`: The port for your service.
- `service.targetPort`: The target port for your service.
- `replicaCount`: The number of replicas for your application.
- `image.registry`: The Docker image registry for your application The value needs to have the '/' at the end and this field is used together with the 'image.repositoryPath' field and 'image.tag' to create the fully URL of the application's docker image. The format is '{{ .Values.image.registry }}{{ .Values.image.repositoryPath }}:{{ .Values.image.tag }}' .
- `image.repositoryPath`: The 'image.repositoryPath' field is used together with the 'image.registry' field and 'image.tag' to create the fully URL of the application's docker image. The format is '{{ .Values.image.registry }}{{ .Values.image.repositoryPath }}:{{ .Values.image.tag }}'
- `image.tag`: The Docker image tag for your application.
- `image.pullPolicy`: The image pull policy.
- `image.pullSecret`: The secret for pulling the Docker image.
- `debug.enabled`: Enable/disable Java remote debugging.
- `debug.port`: The port for Java remote debugging.
- `probes.enabled`: Enable/disable readiness, liveness, and startup probes.
- `probes.readinessProbe`: Configuration for the readiness probe.
- `probes.livenessProbe`: Configuration for the liveness probe.
- `probes.startupProbe`: Configuration for the startup probe.

