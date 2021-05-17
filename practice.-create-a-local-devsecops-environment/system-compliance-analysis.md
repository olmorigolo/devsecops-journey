---
description: With Chef Inspec
---

# System Compliance Analysis

Chef InSpec is a framework for testing and **auditing applications and infrastructure**. Chef InSpec compares the actual state of the system with the desired state defined in a chef recepe written in ruby. Chef InSpec detects violations and displays findings in the form of a report, but puts you in control of remediation.

Inspec Profiles: contain a whole inspec definition.

Controls contain tests \(like in ansible tasks contain the atomic tasks\)

Inspect does not need to be installed on remote machines.

Alternatives to inspect are: puppet, chef, ansible, OpenSCA.

## Installation

wget [https://packages.chef.io/files/stable/inspec/4.18.114/ubuntu/16.04/inspec\_4.18.114-1\_amd64.deb](https://packages.chef.io/files/stable/inspec/4.18.114/ubuntu/16.04/inspec_4.18.114-1_amd64.deb) dpkg -i inspec\_4.18.114-1\_amd64.deb

## Run

On the executing machine, run `echo "StrictHostKeyChecking no" >> ~/.ssh/config`to prevent the ssh agent from prompting YES or NO question. Then run a baseline profile: 

```text
inspec exec 
https://github.com/dev-sec/linux-baseline  # The chosen profile, some linux baseline tests
 -t ssh://root@prod-LQc4TsDQ   # target machine
 -i ~/.ssh/id_rsa    # specify the ssh-key since we are using login in via ssh
 --chef-license accept  # prevent the inspec from prompting YES or NO question
```

## Integration into CD/CI

We want to run Inspec on the prod machine $DEPLOYMENT\_SERVER. On the executing machine \(here our gitlab server\) we need to configure a variable $DEPLOYMENT\_SERVER\_SSH\_KEY and store the ssh key of the prod machine to it.  

```text
inspec:
  stage: prod
  image: hysnsec/inspec
  only:
    - "master"
  environment: production
  before_script:
    - mkdir -p ~/.ssh
    - echo "$DEPLOYMENT_SERVER_SSH_PRIVKEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - ssh-keyscan -H $DEPLOYMENT_SERVER >> ~/.ssh/known_hosts
  script:
    - inspec exec https://github.com/dev-sec/linux-baseline -t ssh://root@$DEPLOYMENT_SERVER -i ~/.ssh/id_rsa --chef-license accept --reporter json:inspec-output.json
  artifacts:
    paths: [inspec-output.json]
    when: always
```

## Create Custom Inspec Profile

Create a folder, enter and initialize a new profile for a ubuntu machine: 

`mkdir inspec-profile && cd inspec-profile`

`inspec init profile ubuntu --chef-license accept`

add some tests to the generated control file: 

```text
cat >> ubuntu/controls/example.rb <<EOL
describe file('/etc/shadow') do
    it { should exist }
    it { should be_file }
    it { should be_owned_by 'root' }
  end
EOL
```

This code basically checks whether shadow file is owned by root or not more rules here: [https://community.chef.io/tools/chef-inspec/](https://community.chef.io/tools/chef-inspec/)

run the profile

`inspec check ubuntu`

run profile against remote machine

 `inspec exec ubuntu -t ssh://root@prod-LQc4TsDQ -i ~/.ssh/id_rsa --chef-license accept`

## Implement PCI/DSS Rules

Chef implementaiton of PCI /DSS rules are defined here [https://www.chef.io/docs/default-source/whitepapers/guidetopcidsscompliance.pdf](https://www.chef.io/docs/default-source/whitepapers/guidetopcidsscompliance.pdf)

We want to implement a verification that the firewall and ssh on the prod machine are correctly configured.

So again create a folder and initialize the files

`mkdir inspec-profile && cd inspec-profile`

`inspec init profile pcidss --chef-license accept` Copy paste the following controls to the example.rb file

```text
control "Firewall-1.0" do
  impact 1.0
  title "Ensure default deny firewall policy."
  %w[INPUT OUTPUT FORWARD].each do |chain|
    describe.one do
      describe iptables do
        it { should have_rule("-P #{chain} DROP") }
      end
      describe iptables do
        it { should have_rule("-P #{chain} REJECT") }
      end
    end
  end
end

control "SSH-1.0" do
  impact 0.8
  title "Ensure SSH uses RSA authentication"
  describe sshd_config do
    its('RSAAuthentication') { should_not eq 'no' }
    its('Protocol') { should eq '2' }
  end
end

```

And we can run this profile by using this command:

```text
inspec exec pcidss -t ssh://root@production-server -i ~/.ssh/id_rsa --chef-license accept
```



