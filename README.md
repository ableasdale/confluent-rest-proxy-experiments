# Confluent ReST Proxy Experiments

Some local testing with Confluent ReST Proxy to provision topics and ACLs.

## Walkthrough

Start the containers:

```bash
docker container prune -f && docker-compose up
```

### Warning about security

Note that the purposes of this repository is to test using ReST Proxy against an existing cluster; we've used a few convenience shortcuts (such as `KAFKA_ALLOW_EVERYONE_IF_NO_ACL_FOUND` and assigning the anonymous user to `KAFKA_SUPER_USERS`).  These are intended for testing - do not use the provided Docker configuration in production without building security into the project.

### Confirm that ReST proxy is running

Get the Cluster ID information using ReST Proxy:

```bash
curl localhost:8082/v3/clusters | jq
```

### List all ACLs for the cluster

Get the list of ACLs

```bash
curl localhost:8082/v3/clusters/ZWe3nnZwTrKSM0aM2doAxQ/acls | jq
```

### Create topics

We're going to create a number of topics:

```bash
for topicName in a b c d e f g h i j k l m n o p q r s t u v w x y z; do
  curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" --data '{"topic_name": "topic_'$topicName'", "partitions_count": 3, "replication_factor": 3}' "http://localhost:8082/v3/clusters/ZWe3nnZwTrKSM0aM2doAxQ/topics"
done 
```

### Assign ACLs

Now let's add ACLs to each of those topics

```bash
for topicName in a b c d e f g h i j k l m n o p q r s t u v w x y z; do
  curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" --data '{ "resource_type": "TOPIC", "resource_name": "'$topicName'", "pattern_type": "LITERAL", "principal": "principalType:principalName", "host": "*", "operation": "READ",  "permission": "ALLOW"}' "http://localhost:8082/v3/clusters/ZWe3nnZwTrKSM0aM2doAxQ/acls"
done
time 
```

Based on some local testing, the call to create the ACLs for multiple topics take less than a minute.

## Structure of the ReST Proxy ACL endpoint

The request allows you to filter by various parameters:

```bash
GET /v3/clusters/ZWe3nnZwTrKSM0aM2doAxQ/acls
?resource_type={resourceType}
&resource_name={resourceName}
&pattern_type={patternType}
&principal={principal}
&host={host}
&operation={operation}
&permission={permission}
```
