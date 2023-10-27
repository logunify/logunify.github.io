# Code Generation

LogUnify offers a code generation service that creates strong typed native SDKs from schemas. We use protobuf as a basis for the generated SDKs, providing an efficient and reliable means of transporting and ensuring consistency of logged events across different platforms and programming languages. Protobuf's binary format is optimized for performance and bandwidth efficiency, while its strong typing and schema evolution capabilities help ensure consistency and compatibility over time. 

Below are the steps to follow to run the code generation:

## Install Docker

The Codegen service requires Docker to be installed in order to run. If you do not already have Docker installed, you can download it from the official Docker website.

## Clone the Codegen Repository

After Docker is installed, you need to clone the Codegen repository from GitHub. You can do this by running the following command: `git clone https://github.com/codegen`.
This will create a local copy of the Codegen repository on your machine. Inside the repository we provide a `scripts` directory which includes serveral utility 
scripts.

## Option 1: Run Codegen from the LogUnify Service
Start the LogUnify service by running the following command:

```bash
 ./scripts/service.sh -s /path/to/schema
```
The script has three parameters that you can use:

- `-s` or `--schema-path`: This is the path to folder containing all the schema files.

Once the service is fully start you should see the following logs:
```
20xx-xx-xx xx:xx:xxx.xxx  INFO 7 --- [main] com.logunify.LogunifyServiceApplication  : Started LogunifyServiceApplication in x.xxx seconds (JVM running for x.xxx)
```
Then you can hit the following api to download the generated code for the specific schema. This api will download the source code directly.
```http
http://localhost:8081/download-source-file/{projectName}/{schemaName}?language={language}" 
```
Or the following api to download the genearted code for all schemas under a project. This api will download the packaged tarball file that includes source files of all the schemas under the project: 
```
http://localhost:8081/ddownload-source-file/{projectName}?language={language}" 
```
Right now for the `language` parameter we support `java`, `swift` and `type_script`.


## Option 2: Run Codegen as a Standalone Utility

Once you have cloned the Codegen repository, you can run the Codegen script located in the scripts directory. The script is called codegen.sh. You can run the script by navigating to the scripts directory and running the following command:

```bash
./scripts/codegen.sh -s /path/to/schema -d /path/to/destination --languages java,swift,type_script
```

This command will generate code from the schema located at `/path/to/schema` and place the generated code in the directory `/path/to/destination`.

The script has three parameters that you can use:

- `-s` or `--schema-path`: This is the path to the schema file or folder containing all the schema files that you want to generate code from. If you are generating code from a single schema file, you can provide the path to that file. If you are generating code from multiple schema files, you can provide the path to the folder containing those files.
- `-d` or `--destination-path`: This is the path to the directory where the generated code will be placed. You need to create this directory before running the Codegen service.
- `--languages`: This specifies the languages of the generated code, right now `java`, `swift` and `type_script` are supported.

### (Optional) Run the Codegen Docker Image Directly

If you prefer, you can also directly call the Codegen Docker image to generate code. You can do this by running the following command:

```bash
docker run -it -v "$SCHEMA_PATH":/app/config -v "$DESTINATION_PATH":/app/generated logunify/codegen:latest -- \
  -s /app/config \
  --logunify_plugin_path /app/codegen/bin/protoc_plugin \
  --destination_path /app/generated \
  --languages java,swift,type_script
```

In this command, you need to replace `$SCHEMA_PATH` and `$DESTINATION_PATH` with the actual paths to your schema file(s) and destination directory, respectively. The schema path and destination path are the same as the `-s` and `-d` parameters in the script.

The --logunify_plugin_path parameter specifies the path to the protoc plugin, you should keep it as is.

That's it! With these steps, you can use the Codegen service to generate code from a schema.
