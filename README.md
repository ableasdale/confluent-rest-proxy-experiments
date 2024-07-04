# Confluent ReST Proxy Experiments

Some local testing with Confluent ReST Proxy.

## Walkthrough

Start the containers:

```bash
docker-compose up -d
```

Confirm that ReST proxy is running:

```bash
curl localhost:8082/v3/clusters | jq
```

Get the Cluster ID information using ReST Proxy

```bash
curl localhost:8082/v3/clusters | jq
```

Get the list of ACLs

```bash
curl localhost:8082/v3/clusters/ZWe3nnZwTrKSM0aM2doAxQ/acls | jq
```

Create some topics

```bash
for topicName in a b c d e f; do
  curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" --data '{"topic_name": "topic_'$topicName'", "partitions_count": 3, "replication_factor": 3}' "http://localhost:8082/v3/clusters/ZWe3nnZwTrKSM0aM2doAxQ/topics"
done 
```

Add ACLs to those topics

```bash
for topicName in a b c d e f; do
  curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" --data '{ \
            "resource_type": "TOPIC", \
            "resource_name": "'$topicName'", \
            "pattern_type": "LITERAL", \
            "principal": "principalType:principalName", \
            "host": "*", \
            "operation": "READ", \
            "permission": "ALLOW" \
        }' "http://localhost:8082/v3/clusters/ZWe3nnZwTrKSM0aM2doAxQ/acls"
done 
```


------


for topicName in a b c d e f; do
  echo "topic_${!topicName}"
done

/v3/clusters/ZWe3nnZwTrKSM0aM2doAxQ/topics
docker container prune -f && docker-compose up
docker container prune && docker-compose up
GET /v3/clusters/ZWe3nnZwTrKSM0aM2doAxQ/acls
?resource_type={resourceType}
&resource_name={resourceName}
&pattern_type={patternType}
&principal={principal}
&host={host}
&operation={operation}
&permission={permission}