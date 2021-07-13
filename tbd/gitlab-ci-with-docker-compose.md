# Gitlab-ci with docker-compose

### Gitlab-ci with docker-compose

I use docker-compose and the following dockerfile to create the gitlab web instance as well as a docker runner on the same machine.

```text
version: '3.9'

services:
  gitlab-web:
    image: gitlab/gitlab-ce:latest
    restart: unless-stopped
    container_name: gitlab-web
    hostname: gitlab-web
    volumes:
      - '$GITLAB_HOME/config:/etc/gitlab'
      - '$GITLAB_HOME/logs:/var/log/gitlab'
      - '$GITLAB_HOME/data:/var/opt/gitlab'
    ports:
      - '2222:22'
      - '8080:80'
      - '443:443'
      - '4567:4567'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        gitlab_rails['initial_root_password'] = '${INITIAL_ROOT_PASSWORD}'
        gitlab_rails['initial_shared_runners_registration_token'] = "${INITIAL_RUNNER_TOKEN}"
        alertmanager['flags'] = {
          'cluster.advertise-address' => "127.0.0.1:9093",
          'web.listen-address' => "localhost:9093",
          'storage.path' => "/var/opt/gitlab/alertmanager/data",
          'config.file' => "/var/opt/gitlab/alertmanager/alertmanager.yml"
        }
    networks:
      - default

  gitlab-runner:
    image: gitlab/gitlab-runner:latest
    container_name: gitlab-runner
    hostname: gitlab-runner
    depends_on:
      - gitlab-web
    volumes:
      - './gitlab-runner-config:/etc/gitlab-runner:Z'
      - '/var/run/docker.sock:/var/run/docker.sock'

networks:
    default:

```

global variables are stored in a .env file:

```text
INITIAL_ROOT_PASSWORD=supersecretpass
INITIAL_RUNNER_TOKEN=o8Yesbgz5hPWVLQqxWF3
GITLAB_HOME=/srv/gitlab
HOST_NAME=gitlab.example.com
```

Run the file with

```text
docker-compose up -d
```

You can observe the status of your gitlab-web instance with 

```text
docker-compose ps
```

```text
docker logs -f gitlab-web
```

If you run into configuration troubles you need to enter the container to make changes to the gitlab.rb file and call gitlab reconfigure

```text
docker container exec -it gitlab-web /bin/bash
$ vi /etc/gitlab/gitlab.rb
```

Exit the container \(CTRL+D\) and check the logs with 

```text
docker logs gitlab-web
```

{% hint style="danger" %}
Pay attention the fix to gitlab.rb is not permanent, reconfigure or restart the container will remove your change and you have to do it again. You need to put the correct configuration into the dockerfile at GITLAB\__OMNIBUS\__CONFIG 
{% endhint %}

More

{% embed url="https://gitlab.com/gitlab-org/omnibus-gitlab/-/issues/3705" %}

Run a docker container as described here [https://docs.gitlab.com/omnibus/docker/](https://docs.gitlab.com/omnibus/docker/)

Another good installation guide is available here: [https://oramind.com/private-cicd-using-gitlab-docker/](https://oramind.com/private-cicd-using-gitlab-docker/)

