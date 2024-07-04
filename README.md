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


------
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