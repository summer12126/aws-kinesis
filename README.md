# AWS Kinesis Data Stream CLI Commands

## Overview

This README provides a structured guide for using AWS CLI commands to interact with Amazon Kinesis Data Streams. The document covers both producer and consumer operations, with examples for both AWS CLI version 1 and version 2.

## Prerequisites

- AWS CLI installed and configured with appropriate permissions
- A Kinesis data stream named "demostream" already created in your AWS account
- Basic understanding of Kinesis concepts (streams, shards, partition keys)

## AWS CLI Version Check

Before starting, you can verify your AWS CLI version with:

```bash
aws --version
```

## Producer Operations

### Adding Records to a Kinesis Stream

#### Using AWS CLI v2

AWS CLI v2 requires the `--cli-binary-format` parameter when sending data:

```bash
# Add a user signup event
aws kinesis put-record --stream-name demostream --partition-key user1 --data "user signup" --cli-binary-format raw-in-base64

# Add a user signin event
aws kinesis put-record --stream-name demostream --partition-key user1 --data "user signin" --cli-binary-format raw-in-base64

# Add incomplete data
aws kinesis put-record --stream-name demostream --partition-key user1 --data "user " --cli-binary-format raw-in-base64
```

#### Using AWS CLI v1

AWS CLI v1 doesn't require the binary format parameter:

```bash
aws kinesis put-record --stream-name demostream --data "user"
```

## Consumer Operations

### Describing a Stream

To get information about your stream, including the number of shards and status:

```bash
aws kinesis describe-stream --stream-name demostream
```

### Reading Data from a Stream

Reading data is a two-step process:

1. Get a shard iterator:

```bash
aws kinesis get-shard-iterator --stream-name demostream --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON
```

2. Use the shard iterator to retrieve records:

```bash
aws kinesis get-records --shard-iterator 
```

## Key Parameters Explained

- **--stream-name**: The name of your Kinesis data stream
- **--partition-key**: Used to determine which shard the data goes to
- **--data**: The actual payload to send to the stream
- **--cli-binary-format**: Required in CLI v2 to specify how binary data is encoded
- **--shard-id**: Identifier for the specific shard you want to read from
- **--shard-iterator-type**: Determines where to start reading from:
  - TRIM_HORIZON: Oldest available records
  - LATEST: Most recent records
  - AT_SEQUENCE_NUMBER/AFTER_SEQUENCE_NUMBER: Start from a specific position

## Common Issues

- Ensure proper formatting of the `--cli-binary-format` parameter in CLI v2
- The shard iterator expires after a short period, so you may need to request a new one
- Check stream status before attempting to read or write data

---
Answer from Perplexity: pplx.ai/share
