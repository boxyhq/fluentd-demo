# Fluentd Docker Demo

- Input: HTTP endpoint http://127.0.0.1:9880/sample.test
- Output: MongoDB

## Install

```bash
cd fluentd-demo
```

```bash
docker-compose up --build
```

## Log Ingestion

```
curl -X POST -d 'json={
   "actor":"john-doe",
   "actor_type":"user",
   "group":"boxyhq",
   "where":"127.0.0.1",
   "where_type":"ip",
   "when":"2021-05-18T20:53:39+01:00",
   "target":"user.login",
   "target_id":"10",
   "action":"login",
   "action_type":"U",
   "name":"user.login",
   "description":"This is a login event",
   "metadata":{
      "foo":"bar",
      "hey":"you"
   }
}' http://127.0.0.1:9880/sample.test
```

## MongoDB Document Sample

```
{
   "_id":"ObjectId(""620407d2a9a841001d0cc068"")",
   "actor":"john-doe",
   "actor_type":"user",
   "group":"boxyhq",
   "where":"127.0.0.1",
   "where_type":"ip",
   "when":"2021-05-18T20:53:39 01:00",
   "target":"user.login",
   "target_id":"10",
   "action":"login",
   "action_type":"U",
   "name":"user.login",
   "description":"This is a login event",
   "metadata":{
      "foo":"bar",
      "hey":"you"
   },
   "time":"ISODate(""2022-02-09T18:28:31.934Z"")"
}
```

## Apache Bench

```bash
ab -n 10000 -c 200 -p data.json -T application/json http://127.0.0.1:9880/sample.test
```