# Self-hosted DevOps CD/CI platforms

## Gitlab CI

I recommend gitlab CI if you want to experiment how to integrate different tools into a CD/CI. Gitlab has a professional version, so the private installation is limited in features. Run a docker container as described here [https://docs.gitlab.com/omnibus/docker/](https://docs.gitlab.com/omnibus/docker/)

I use docker-compose and the following dockerfile

```text
web:
  image: 'gitlab/gitlab-ee:latest'
  restart: always
  hostname: 'gitlab.example.local'
  ports:
    - '80:80'
    - '443:443'
    - '22:22'
  volumes:
    - '$GITLAB_HOME/config:/etc/gitlab'
    - '$GITLAB_HOME/logs:/var/log/gitlab'
    - '$GITLAB_HOME/data:/var/opt/gitlab'
```

When starting the container with `docker-compose up -d`I ran into the following  problem [https://gitlab.com/gitlab-org/omnibus-gitlab/-/issues/3705](https://gitlab.com/gitlab-org/omnibus-gitlab/-/issues/3705).

### Installation Fix

To fix it, go into the container with `docker exec -it gitlab_web /bin/bash` then do one of the following:

Solution 1: change the runner file

`vi /opt/gitlab/sv/alertmanager/run` and add the line `--cluster.advertise-address=127.0.0.1:9093`as described in the article. 

Solution 2: add alertmanager options to the gitlab.rb config file

```text
└─$ sudo docker exec -it gitlab_web_1 /bin/bash
root@gitlab:/# vi /var/opt/gitlab/alertmanager/alertmanager.yml
root@gitlab:/# vi /etc/gitlab/gitlab.rb
```

Find "Prometheurs Alertmanager" section and copy paste the following code

```text
 alertmanager['flags'] = {
   'cluster.advertise-address' => "127.0.0.1:9093",
   'web.listen-address' => "localhost:9093",
   'storage.path' => "/var/opt/gitlab/alertmanager/data",
   'config.file' => "/var/opt/gitlab/alertmanager/alertmanager.yml"
 }

```

Exit the container \(CTRL+D\) and check the logs with 

```text
sudo docker logs gitlab_web
```

You should be able to open gitlab.example.local in your browser.

{% hint style="danger" %}
Pay attention the fix is not permanent, reconfigure or restart the container will remove your change and you have to do it again.
{% endhint %}

### Push your first vulnerable app to gitlab

1. Create a namespace \(group\) and a project in your gitlab instance
2. Get your public ssh key and enter it in the ssh key sections of your user in gitlab `cat ~/.ssh/id_rsa.pub`
3. Clone a vulnerable app from github `git clone` [`https://github.com/stamparm/DSVW.git`](https://github.com/stamparm/DSVW.git)\`\`
4. Push it to you local gitlab instance `git push --set-upstream git@localhost:vulnlab/dsvw.git`

### Add runners

Docs [https://docs.gitlab.com/runner/](https://docs.gitlab.com/runner/)

#### With docker-compose on the local machine

People are asking fo a docker-compose file that creates the gitlab instance and registers runners at the same time. Here are come solutions: [https://gitlab.com/gitlab-org/gitlab/-/issues/23911](https://gitlab.com/gitlab-org/gitlab/-/issues/23911)

#### With amazon aws kubenetes service on remote machines

TBD I'll write about this in the future.

#### With docker on the local machine

Add a docker container with the gitlab-runner image

```text
docker run -d --name gitlab-runner --restart always \
     -v /srv/gitlab-runner/config:/etc/gitlab-runner \
     -v /var/run/docker.sock:/var/run/docker.sock \
     gitlab/gitlab-runner:latest
```

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

