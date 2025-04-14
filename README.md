# s3-mcp

[![smithery badge](https://smithery.ai/badge/@ashgw/s3-mcp)](https://smithery.ai/server/@ashgw/s3-mcp)


## Overview

This server enables you to:

- Manage S3 buckets and objects
- Handle lifecycle configurations
- Set and retrieve object tags
- Manage bucket policies
- Configure CORS settings

## Installation

There are multiple ways to use this server depending on your setup.

### Installing via Smithery

To install s3-mcp for Claude Desktop automatically via [Smithery](https://smithery.ai/server/@ashgw/s3-mcp):

```bash
npx -y @smithery/cli install @ashgw/s3-mcp --client claude
```

### Cursor (recommended)

Add this to your Cursor MCP configuration:

```json
{
  "mcpServers": {
    "s3-mcp": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "AWS_SECRET_ACCESS_KEY=your_access_key",
        "-e",
        "AWS_ACCESS_KEY_ID=your_access_key",
        "-e",
        "AWS_REGION=your_region",
        "ashgw/s3-mcp:latest"
      ]
    }
  }
}
```

> If you prefer pinning to a specific Docker image build (e.g., 20250413-165732), use that tag instead of `latest`. Browse available versions on [Docker Hub](https://hub.docker.com/r/ashgw/s3-mcp/tags).

Cursor will route that request through the MCP server automatically.

Make sure it's green and all the tools are available.

![image](https://github.com/user-attachments/assets/6b37044d-e6f3-447a-8809-4431822c0731)

---

### Docker (manual)

You can run the S3 MCP server manually via Docker:

```bash
docker run --rm -it \
  -e AWS_ACCESS_KEY_ID=your_access_key \
  -e AWS_SECRET_ACCESS_KEY=your_secret_key \
  -e AWS_REGION=your_region \
  ashgw/s3-mcp
```

This uses the pre-built image published at [ashgw/s3-mcp](https://hub.docker.com/repository/docker/ashgw/s3-mcp).

---

### Repo

Clone the repository and `cd` into it, then build with:

```bash
docker build -t s3-mcp .
```

Then run with:

```bash
docker run --rm -e AWS_ACCESS_KEY_ID=your_access_key -e AWS_SECRET_ACCESS_KEY=your_secret_key -e AWS_REGION=your_region s3-mcp
```

### Environment Variables

Set the following environment variables for AWS credentials:

```bash
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key
export AWS_REGION=your_region
```

## Features

- **List Buckets**: Retrieve a list of all S3 buckets.
- **Create Bucket**: Create a new S3 bucket with optional configurations.
- **List Objects**: List all objects in a specified bucket.
- **Get Object**: Retrieve the content of a specified object.
- **Put Object**: Upload an object to a specified bucket.
- **Delete Object**: Remove an object from a specified bucket.
- **Generate Presigned URL**: Create a presigned URL for accessing or uploading an object.
- **Set Bucket Policy**: Update or set a policy for a specified bucket.
- **Get Bucket Policy**: Retrieve the current policy for a specified bucket.
- **Delete Bucket Policy**: Remove the current policy for a specified bucket.
- **Lifecycle Configuration**: Manage lifecycle rules for S3 buckets.
- **Object Tagging**: Set and retrieve tags for S3 objects.
- **CORS Configuration**: Get and set CORS rules for a bucket.
- **Copy Object**: Copy an object from one location to another within S3.
- **Download File to Local**: Download a file from a specified S3 bucket to a local path.
- **Upload Local File**: Upload a local file to a specified S3 bucket.

## Few Usage Examples

### List Buckets

```python
response = await tool("list_buckets")
print(response)
```

### Create a Bucket

```python
response = await tool("create_bucket", {
    "bucket_name": "my-new-bucket",
    "region": "us-west-1",
    "config": {
        "blockPublicAccess": {
            "BlockPublicAcls": True,
            "IgnorePublicAcls": True,
            "BlockPublicPolicy": True,
            "RestrictPublicBuckets": True
        },
        "versioning": True,
        "encryption": "AES256"
    }
})
print(response)
```

### Upload a File

```python
response = await tool("upload_local_file", {
    "bucket_name": "my-new-bucket",
    "local_path": "/path/to/local/file.txt",
    "key": "file.txt"
})
print(response)
```

### Get Object Tags

```python
response = await tool("get_object_tagging", {
    "bucket_name": "my-new-bucket",
    "key": "file.txt"
})
print(response)
```

## License

[MIT](/LICENSE)
