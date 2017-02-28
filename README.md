
# Druid(v0.8.3) Docker Image(with [druid-datasketches](http://druid.io/docs/latest/development/extensions-core/datasketches-aggregators.html) extension and [caravel](https://github.com/airbnb/superset) included)

## Run a simple Druid cluster

Command to run the docker image :
```sh
docker run -t -d --net=host dev-dockerhub.corp.inmobi.com:7000/deepak_rai/docker-druid:a580aadf846e-caravel-42
```

Druid UI : http://localhost:8081/#/

Caravel UI(credentials - admin/admin) : http://localhost:8088/

## Build Druid Docker Image

Jenkins job for building this docker : https://build-dev.corp.inmobi.com/job/QA_druid_docker_Dev_master_Docker/

To build the docker image yourself

```sh
git clone git@github.corp.inmobi.com:deepak-rai/docker-druid.git
cd docker-druid
docker build -t example-cluster .
```

## Ingesting test data

- Test data in [sample_data](files/sample_data/) is copied into Docker image at `/home/inmobi/sample_data/`.
- To ingest this test data into druid, run following from within the docker:
```sh
root@geo-qa1002:/# curl -X 'POST' -H 'Content-Type:application/json' -d @/home/inmobi/sample_data/spec.json  http://localhost:8090/druid/indexer/v1/task
{"task":"index_hadoop_test_data_2017-02-25T04:01:12.810Z"}
```
- Sample query command(to be run from within the docker):
```sh
root@geo-qa1002:/# curl -X 'POST' -H 'Content-Type:application/json' -d @/home/inmobi/sample_data/query.json http://localhost:8082/druid/v2/?pretty
[ {
  "version" : "v1",
  "timestamp" : "2017-01-20T00:00:00.000Z",
  "event" : {
    "4000_unique_users" : 2.0
  }
} ]root@geo-qa1002:/#
```

