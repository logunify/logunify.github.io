# Running Logunify Backend Service

There are three ways to run the logunify backennd service. You can either run the docker image on your host machine, using kubernetes to run the docker image, or run the java service directly (not recommended).

## Run the Service

### Running the Docker Image

One way to run the Logunify backend service is by using the Docker image. The Docker image can be built locally or pulled from Dockerhub. To run the image from Dockerhub, you can use the `scripts/service.sh` script [TODO] with the `-s` option, followed by the schema path. For example: `service.sh -s $SCHEMA_PATH`

To build and run the local Docker image, you can use the `scripts/build_and_run.sh` script. This script will build the image and then start the container. For example: `build_and_run.sh -s $SCHEMA_PATH`

### Running on Kubernetes

Another option to run the Logunify backend service is on Kubernetes. You can use the Docker image provided above and deploy it on a Kubernetes cluster. This can be done using Kubernetes YAML files and deploying them using `kubectl`.

You canfind an example kubernetes deployment config below.

There are two yaml schema configurations, `config1.yaml` and `config2.yaml` in this example. These configurations are mounted to the `/app/config` folder of the `logunify` container. There is a `logunify-service` k8s service exposes the service to external users, and a horizontal auto scaler for auto scaling.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: configmap1
data:
  config1.yaml: |
    # Configuration for config1
    key1: value1
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: configmap2
data:
  config2.yaml: |
    # Configuration for config2
    key2: value2
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: logunify-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: logunify
  template:
    metadata:
      labels:
        app: logunify
    spec:
      containers:
        - name: logunify
          image: logunify/logunify:latest
          resources:
            limits:
              cpu: 1
              memory: 2Gi
          volumeMounts:
            - name: configmap1-volume
              mountPath: /app/config/config1.yaml
              subPath: config1.yaml
            - name: configmap2-volume
              mountPath: /app/config/config2.yaml
              subPath: config2.yaml
          ports:
            - containerPort: 8081
      volumes:
        - name: configmap1-volume
          configMap:
            name: configmap1
        - name: configmap2-volume
          configMap:
            name: configmap2
---
apiVersion: v1
kind: Service
metadata:
  name: logunify-service
spec:
  selector:
    app: logunify
  ports:
    - name: http
      port: 8081
      targetPort: 8081
---
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: logunify-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: logunify-deployment
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        targetAverageUtilization: 40
```

### Running the Java Service Directly

The last option is to run the Java service directly. This method is not recommended, as it is difficult to install all dependencies. However, if you want to run the Java service directly, you can find the setup in the [Dockerfile](TODO).

## Verification of Service

> Note: Replace baseurl with the actual URL of the service and your_auth_token with the authentication token of the user.

### Verification of Startup:

To check if the service has been successfully started, follow the below steps:

- Open a command prompt or terminal.
- Enter the following command: `curl -X GET baseurl/actuator/health/readiness`
- The output should be `{"status":"UP"}`.
- Similarly, enter the following command: `curl -X GET baseurl/actuator/health/liveness`
- The output should be `{"status":"UP"}`.

### Verification of Loaded Schemas:

To check if the schemas have been successfully loaded, follow the below steps:

- Open a command prompt or terminal.
- Enter the following command: `curl -X GET baseurl/api/info/schemas`
- The output should be a list of schemas in the server, like `[testProject/UserActivity, testProject/UserActivity2]`.

### Verification of Connected Integrations

To check if all integrations have been successfully connected, follow the below steps:

- Open a command prompt or terminal.
- Enter the following command: `curl -X GET baseurl/api/info/sinks`
- The output should be a list of all live integrations, like
  ```json
  {
    "testProject/UserActivity": ["BigQuery/fit-aloe-293300.pipelineTest.userActivity"],
    "testProject/UserActivity2": ["BigQuery/fit-aloe-293300.pipelineTest.userActivity3"]
  }
  ```

### Verification of Tables Created

LogUnify automatically creates needed resources like bigquery tables. After the service started, please go to bigquery to verify the sink table has been succesfully created.
