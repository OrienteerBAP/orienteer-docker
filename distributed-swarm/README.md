### Launch Orienteer in Docker Swarm

`docker-compose.yml` allows easy launch Orienteer in distributed mode inside Docker Swarm.

| File Name                                 | Description                                        |
|-------------------------------------------|----------------------------------------------------|
| config/hazelcast-docker.xml               | Hazelcast config file for use it inside Docker     |
| config/events.json                        | OrientDB events config file                        |
| config/default-distributed-db-config.json | OrientDB config file for OrientDb distributed mode |
| config/backups.json                       | OrientDB backups config file                       |
| config/automatic-backup.json              | OrientDB automatic backup config file              |


> For understand how to work with OrientDB configuration for distributed mode, please see [OrientDB Scaling](https://orientdb.com/docs/last/Distributed-Architecture.html "OrientDB Scaling").


#### Usage
For use current `docker-compose.yml`, please use this guide.
1. Your `dockerd` must have enabled remote API
    Orienteer uses [hazelcast-docker-swarm-discovery-spi](https://github.com/bitsofinfo/hazelcast-docker-swarm-discovery-spi) for detect Orienteer nodes inside Docker Swarm cluster, so `dockerd` must have enabled remote API for HTTP or HTTPS communication.
    * See about Docker deamon socket [Docker docs](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-socket-option)
    * Modify systemd config for Docker deamon. [Example](https://success.docker.com/article/how-do-i-enable-the-remote-api-for-dockerd)
    * For using TLS with Docker deamon socket see [Docker docs](https://docs.docker.com/engine/security/https/)
2. Initialize Docker Swarm manager
    * Execute `docker swarm init --advertise-addr ${host}`, where `${host}` is ip address of machine where command is executes
3. Modify `docker-compose.yml`
    * Set environment variable `DOCKER_HOST` to `http://${host}:${port}`, where `${host}` is ip address of Docker Swarm manager (ip address from point 2), `${port}` is port where was bound Docker deamon socket (see point 1).
    For example: `DOCKER_HOST=http://192.168.2.110:2375`
    * Set property `swarmMgrUri` to `http://${host}:${port}`. See previous point.
    For example: `-DswarmMgrUri=http://192.168.2.110:2375`
4. Deploy Docker stack with specified `docker-compose.yml`
    * Execute `docker stack deploy --compose-file docker-compose.yml orienteer`

> For more options see [hazelcast-docker-swarm-discovery-spi](https://github.com/bitsofinfo/hazelcast-docker-swarm-discovery-spi) and [Orienteeer](https://github.com/OrienteerBAP/Orienteer)
