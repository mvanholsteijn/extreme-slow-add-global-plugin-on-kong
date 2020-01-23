# extreme slow add plugin on Kong 1.4.3

Adding a plugin to Kong `1.4.3 on a freshly initialized database is very slow (~30s).


to reproduce
============
```
git clone https://github.com/mvanholsteijn/extreme-slow-add-global-plugin-on-kong.git
cd extreme-slow-add-global-plugin-on-kong
export KONG_DOCKER_TAG=kong:1.4.3 
./reproduce
```
This will result in the following output:
```
 ./reproduce
Pulling db              ... done
Pulling kong-migrations ... done
Pulling kong            ... done
Pulling secure-kong-api ... done
Creating network "slow-kong_kong-net" with the default driver
Creating slow-kong_db_1 ... done
Error: [PostgreSQL error] failed to retrieve PostgreSQL server_version_num: connection refused

  Run with --v (verbose) or --vv (debug) for more details
slow-kong_db_1 is up-to-date
Creating slow-kong_kong-migrations_1 ... done
Creating slow-kong_kong_1            ... done
INFO: waiting 5s for kong to be healthy, now starting
INFO: waiting 5s for kong to be healthy, now starting
slow-kong_db_1 is up-to-date
Starting slow-kong_kong-migrations_1 ... done
slow-kong_kong_1 is up-to-date
Creating slow-kong_secure-kong-api_1 ... done
Attaching to slow-kong_secure-kong-api_1
secure-kong-api_1  | INFO: adding plugin response-transformer to plugins
secure-kong-api_1  | real	0m29.992s
secure-kong-api_1  | user	0m0.005s
secure-kong-api_1  | sys	0m0.006s
```

This problem did not exist in Kong 1.3.1.

```
docker-compose down
export KONG_DOCKER_TAG=kong:1.3.1
./reproduce
```
As you can see from the output, the add plugin takes just under 0.2s.
```
Pulling db              ... done
Pulling kong-migrations ... done
Pulling kong            ... done
Pulling secure-kong-api ... done
Creating network "slow-kong_kong-net" with the default driver
Creating slow-kong_db_1 ... done
Error: [PostgreSQL error] failed to retrieve server_version_num: connection refused

  Run with --v (verbose) or --vv (debug) for more details
slow-kong_db_1 is up-to-date
Creating slow-kong_kong-migrations_1 ... done
Creating slow-kong_kong_1            ... done
INFO: waiting 5s for kong to be healthy, now starting
INFO: waiting 5s for kong to be healthy, now starting
slow-kong_db_1 is up-to-date
Starting slow-kong_kong-migrations_1 ... done
slow-kong_kong_1 is up-to-date
Creating slow-kong_secure-kong-api_1 ... done
Attaching to slow-kong_secure-kong-api_1
secure-kong-api_1  | INFO: adding plugin response-transformer to plugins
secure-kong-api_1  | 
secure-kong-api_1  | real	0m0.019s
secure-kong-api_1  | user	0m0.003s
secure-kong-api_1  | sys	0m0.001s
secure-kong-api_1  | INFO: adding plugin datadog to plugins
secure-kong-api_1  | 
secure-kong-api_1  | real	0m0.012s
secure-kong-api_1  | user	0m0.003s
secure-kong-api_1  | sys	0m0.001s
slow-kong_secure-kong-api_1 exited with code 0
```
