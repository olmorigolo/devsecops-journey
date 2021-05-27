# Self-hosted DevOps CD/CI platforms

## Gitlab CI

I recommend gitlab CI if you want to experiment how to integrate different tools into a CD/CI. Gitlab has a professional version, so the private installation is limited in features. 

Before installing anything on your local machine or VM check the system requirements here [https://docs.gitlab.com/ee/install/requirements.html](https://docs.gitlab.com/ee/install/requirements.html). 

### Gitlab-ci on a virtual machine

{% hint style="success" %}
Recommended solution
{% endhint %}

Install your own gitlab server on a virtual machine as described in [https://embeddedinventor.com/complete-guide-to-setting-up-gitlab-locally-on-mac/\#STEP4\_Create\_and\_edit\_gitlab\_ciyml\_to\_run\_the\_above\_script](https://embeddedinventor.com/complete-guide-to-setting-up-gitlab-locally-on-mac/#STEP4_Create_and_edit_gitlab_ciyml_to_run_the_above_script)

Register your local machine as a runner and run it with `gitlab-runner --debug run`

to see if it connects to the server. Why should you register your local computer as a runner? If your build steps run locally, then they will also run successully with your local gitlab runner. Don't forget to try everything before putting it into a pipeline. Also test if your docker container runs sucessfully on you local machien before setting it up in the pipeline.

Thank you embedded inventor for the detailed tutorial!

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

### Add runners

Docs [https://docs.gitlab.com/runner/](https://docs.gitlab.com/runner/)

#### With docker-compose on the local machine

There is already a running instance thanks to our dockerfile. We need to enter the container and register the runner with: 

```text
docker container exec -it gitlab-runner /bin/bash
$ 
gitlab-runner register \
  --non-interactive \
  --locked=false \
  --description=local \
  --url=https://localhost \
  --registration-token=o8Yesbgz5hPWVLQqxWF3 \
  --executor=docker \
  --docker-image=debian \
  --docker-volumes "/var/run/docker.sock:/var/run/docker.sock"
```

I found the solution here: [https://gitlab.com/gitlab-org/gitlab/-/issues/23911](https://gitlab.com/gitlab-org/gitlab/-/issues/23911)

#### With amazon aws kubenetes service on remote machines

TBD I'll write about this in the future.

#### With docker on the local machine

Add a docker container with the gitlab-runner image. See [https://docs.gitlab.com/runner/](https://docs.gitlab.com/runner/) for more info. 

```text
docker run -d --name gitlab-runner --restart always \
     -v /srv/gitlab-runner/config:/etc/gitlab-runner \
     -v /var/run/docker.sock:/var/run/docker.sock \
     gitlab/gitlab-runner:latest
```

### Push a vulnerable app to gitlab

1. Create a namespace \(group\) and call it vulnlab, create a project in your gitlab instance with name dsvw
2. Read your public ssh key and enter it in the ssh key sections of your user in gitlab `cat ~/.ssh/id_rsa.pub`
3. Clone a vulnerable app from github `git clone` [`https://github.com/stamparm/DSVW.git`](https://github.com/stamparm/DSVW.git)\`\`
4. Push it to you local gitlab instance `git push --set-upstream git@localhost:vulnlab/dsvw.git`

\*\*\*\*🏁 **Start building your pipeline!**

### More

{% embed url="https://gitlab.com/gitlab-org/omnibus-gitlab/-/issues/3705" %}

Run a docker container as described here [https://docs.gitlab.com/omnibus/docker/](https://docs.gitlab.com/omnibus/docker/)

Another good installation guide is available here: [https://oramind.com/private-cicd-using-gitlab-docker/](https://oramind.com/private-cicd-using-gitlab-docker/)

## Jenkins CI

I recommend jenkins if you want to learn how to configure your CD/CI server in detail. Jenkins is completely free of charge and open source. **No sign-up, no limits.** It is more difficult to maintain than the other platforms. Jenkinsfiles that describe the pipeline workflow are less human friendly than the yaml description files used by many other systems.

run the docker container described here [https://www.jenkins.io/doc/book/installing/docker/](https://www.jenkins.io/doc/book/installing/docker/)

## Travis CI

You to sign-up for the free version.

Sales arguments for using travis over jenkins: [https://travis-ci.com/travisci-vs-jenkins](https://travis-ci.com/travisci-vs-jenkins)

## Github Actions

If your code is hosted on **github** in a  **public repository** you can use the free version of Github actions **without limits**. You don't need to install your own CD/CI server. I recommend this option if you want to test some tools and quickly get their output for further analysis.

You need to **sign-up** an account in order to use it.

{% embed url="https://github.com/features/actions" %}

For private repos there is the following limit as of mai 2021:

* 2,000 automation minutes/month
* 500MB of Packages storage

