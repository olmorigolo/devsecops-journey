# Self-hosted DevOps CD/CI platforms

## Gitlab CI

I recommend gitlab CI if you want to experiment how to integrate different tools into a CD/CI. Gitlab has a professional version, so the private installation is limited in features.&#x20;

Before installing anything on your local machine or VM check the system requirements here [https://docs.gitlab.com/ee/install/requirements.html](https://docs.gitlab.com/ee/install/requirements.html).&#x20;

### Gitlab-ci on a virtual machine

{% hint style="success" %}
Recommended solution
{% endhint %}

Install your own gitlab server on a virtual machine as described in [https://embeddedinventor.com/complete-guide-to-setting-up-gitlab-locally-on-mac/#STEP4\_Create\_and\_edit\_gitlab\_ciyml\_to\_run\_the\_above\_script](https://embeddedinventor.com/complete-guide-to-setting-up-gitlab-locally-on-mac/#STEP4\_Create\_and\_edit\_gitlab\_ciyml\_to\_run\_the\_above\_script)

Register your local machine as a runner and run it with `gitlab-runner --debug run` to see if it connects to the server. Why should you register your local computer as a runner? If your build steps run locally, then they will also run successfully with your local gitlab runner. Don't forget to try everything before putting it into a pipeline. Also test if your docker container runs sucessfully on you local machien before setting it up in the pipeline.

Thank you embedded inventor for the detailed tutorial!

### Add runners

Docs [https://docs.gitlab.com/runner/](https://docs.gitlab.com/runner/)

#### With docker-compose on the local machine

There is already a running instance thanks to our dockerfile. We need to enter the container and register the runner with:&#x20;

```
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

#### With docker on the local machine

Add a docker container with the gitlab-runner image. See [https://docs.gitlab.com/runner/](https://docs.gitlab.com/runner/) for more info.&#x20;

```
docker run -d --name gitlab-runner --restart always \
     -v /srv/gitlab-runner/config:/etc/gitlab-runner \
     -v /var/run/docker.sock:/var/run/docker.sock \
     gitlab/gitlab-runner:latest
```

### Push a vulnerable app to gitlab

1. Create a namespace (group) and call it vulnlab, create a project in your gitlab instance with name dsvw
2. Read your public ssh key and enter it in the ssh key sections of your user in gitlab `cat ~/.ssh/id_rsa.pub`
3. Clone a vulnerable app from github (see [Vulnerable Web Apps](../../web-application-security-testing/self-hosted-training-lab/vulnerable-web-apps.md)), for instance `git clone` [`https://github.com/stamparm/DSVW.git`](https://github.com/stamparm/DSVW.git) ``&#x20;
4. Push it to you local gitlab instance `git push --set-upstream git@localhost:vulnlab/dsvw.git`

****:checkered\_flag: **Start building your pipeline!**

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
