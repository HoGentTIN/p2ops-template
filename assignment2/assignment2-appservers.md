# Assignment 2: Automating Linux server setup

## Assignment: Linux Web Application Servers

Several clients ask us to host their **web applications**. Until now, we have always done the installation and configuration of webservers manually. Due to the growing demandm, this is no longer sustainable. The intention is to develop a kind of "template" so that we can set up a new server much faster. It should be configured to run an application without any manual intervention. To make it easier for web application developers to set up a **test environment** on their own laptop, the first step is to create VirtualBox VMs. Ultimately, the intention is to reproduce these in the **cloud**.

We split this assignment up into different milestones

### M1. Local test environment

- The VMs are created with [Vagrant](https://www.vagrantup.com/). Your repository already has a starting environment that you can use to get started. You don't need to download Linux installation ISOs: installation of the OS is automated by Vagrant.
- A first setup is the classic LAMP stack (Linux + Apache + MySQL + PHP) or a variant thereof. This consists of the following components (freely selectable by the team):
    Linux: [CentOS 7](https://app.vagrantup.com/bento/boxes/centos-7.9), [CentOS 8](https://app.vagrantup.com/bento/boxes/centos-8.3) or [Fedora 33 Server](https://app.vagrantup.com/bento/boxes/fedora-33) (without graphical environment)
    - Web server: Apache or nginx
    Database: MariaDB, MySQL or PostgreSQL
    - Web application: Drupal, Wordpress, MediaWiki or any other PHP application of your choice (as long as it uses a database)
- The entire setup is automated with two Bash scripts.
    - The first script installs and configures the "application platform", i.e. the database and web server
    - The second script takes care of the installation of the web application (and any dependencies). If the customer wants to install another PHP application, they can modify this script.
- Make the script configurable. For example, the user must be able to choose the name of the database and passwords himself. Use a configuration file for all settings so that the customer does not have to modify the installation script themselves.
- Have an eye for security:
    - SELinux is on (Enforcing)
    - There is a firewall that only allows the necessary traffic (SSH, HTTP, HTTPS)
    - ...
- Also have an eye for programming style. Apply the techniques you learned in the course Operating Systems: Linux. Those who want to go a step further can watch [this presentation about Bash scripting](https://gitpitch.com/bertvv/presentation-clean-bash).

**Deliverables:**

- Vagrant environment with 1 VM
- Installation script web server + database
- Installation script web application
- Technical documentation:
    - How can a team member (or the facilitator) reproduce the setup without help?
    - Cheat sheet: what are the main commands to manage the VM and to troubleshoot problems?
- User Documentation: How can a PHP developer boot without help configuring the VM?
- Test plan and report

### M2. Monitoring

- You need a monitoring system to monitor the proper functioning of a production server. Modify the installation script to activate [Cockpit](https://cockpit-project.org/) on the VM.
- Run a load test on the web server from the physical system. For example, use [HTTP Perf](https://github.com/httperf/httperf) or another load testing tool. Follow the behavior of the web server via Cockpit. From what load is the server slowing down? When will it fail completely? Which system resource is overloaded first: RAM, CPU, Disk I/O, Network I/O, ...?

**Deliverables:**

- Custom installation script
- Customized technical documentation
- Test plan and report of the load tests

### M3. Production environment

The intention is to be able to set up servers with *exactly the same* configuration in a **production environment**. We are thinking of outsourcing this via an Infrastructure as a Service/public cloud provider.

The following Public Cloud providers give free credits via the [Github Student Pack](https://education.github.com/pack) or via discount codes:

- [Digital Ocean](https://www.digitalocean.com/)
- [Amazon Web Services Educate](https://aws.amazon.com/education/awseducate/)
- [Azure](https://aka.ms/devtoolsforteaching) - Login with HOGENT account
- [Linode](https://linode.com/) - $ 100 credit through [ad](https://linode.com/darknet) on the [Darknet Diaries](https://darknetdiaries.com/) podcast

Choose one of these platforms and reproduce the VirtualBox test environment in the cloud. The creation and basic installation of this VM does not have to be automated (since cloud platforms typically have their own basic OS images), but the further configuration (via the written scripts) should! If your scripts are sufficiently configurable, we expect that only a minimum of customization is required. Make sure the scripts are reusable both locally and on the cloud platform.

Use the functionalities of the cloud environment for monitoring. We do not recommend to perform load tests on cloud platforms because then your credits will be used up quickly!

Also pay attention to security here: in addition to the suggestions given earlier, it is best not to use weak passwords in a production environment!

**Deliverables:**

- Customized installation scripts so that they can be used both locally and in the chosen cloud environment
- Technical documentation: how can a team member reproduce the setup?
- User documentation: how can a customer set up a web application server in a production environment?
- Test plan and report

### M4. Container virtualization

More and more companies are switching from the classic [VM hypervisor](https://en.wikipedia.org/wiki/Hypervisor) to [container virtualization](https://en.wikipedia.org/wiki/OS-level_virtualization). In this market segment, [Docker](https://www.docker.com/) is a product that has become extremely popular in a short time. The hype surrounding Docker was fierce, but is now cooling off. Container virtualization will undoubtedly continue to be important, but multiple players have emerged. The intention is to experiment with these technologies to investigate whether it is interesting to offer your customers support for this.

- Set up a new VM via VirtualBox on which you install a container virtualization platform: CentOS 8 + Podman or CentOS 7 + Docker.
- Install Cockpit with the container monitoring plugin.
- Set up a similar environment to the first VM: a PHP application with a database backend. You will need at least 2 containers: one for the database, one for the web server + application. Automating the setup of multiple containers that work together is often done with Docker Compose. There is undoubtedly an alternative in the Podman ecosystem.
- Make sure the website can be viewed from the physical system through the IP address of the VM and normal ports for web server traffic. You will need to set up port forwarding rules to forward network traffic on those ports to the appropriate container.
- Repeat the load tests from the previous iteration. Does this setup behave fundamentally differently? Can the web server handle more traffic or not? Where is the bottleneck now?

Initially, we do not plan to roll this out in a public cloud environment or to offer it to customers. Documentation for end users is therefore not necessary here.

**Deliverables:**

- Installation scripts VM, containers
- Technical documentation:
    - How can a team member reproduce the setup?
    - Completed cheat sheet: most important commands for working with Docker/Podman
- Test plan and report preparation + load tests

### Acceptance criteria

- It must be easy for an application developer to set up a local **test environment** for running a web application with database backend.
    - You demonstrate this by giving a demo of a web application that runs on the platform
    - Also show the results of the load tests
- Setting up these servers must be **exactly reproducible**. To be truly usable at scale, you need to automate the installation process. A team member must be able, without manual intervention, to set up the whole setup *from scratch* with the command `vagrant up`
- Attention has been paid to **reusability**.
    - The scripts can be used on different types of systems: within the VirtualBox test environment on your desktop, within one of the proposed cloud platforms.
    - Settings specific to an application (e.g. database username and password) are configurable. So avoid hard-coded data between the code, but use variables where possible.
    - Distinguish between the installation script for the *platform* (which is reusable) and the web application itself (defined by the user)
- A proof-of-concept has been set up with a **public cloud platform** (either from the specified providers / products or another after agreement with the supervisors).
- Show a demo of the **container virtualization platform** and the load tests, reflect on the differences in the results
