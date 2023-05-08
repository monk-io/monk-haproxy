# Monk & HAproxy

This repository contains Monk.io template to deploy HAproxy system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Start

Set up Monk - [https://docs.monk.io/docs/monk-in-10/](https://docs.monk.io/docs/monk-in-10/)

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk HAproxy repository

In order to load templates and change configuration simply use below commands:

```bash
git clone https://github.com/monk-io/monk-haproxy

# and change directory to the monk-haproxy/single-haproxy template folder
cd monk-haproxy/single-haproxy
```

## Configuration

You can add/remove configuration of the template.

### HAproxy configuration file

You can find configuration file (haproxy.cfg) in `/files` directory in repository and can edit before the running kit.

| Configuration File | Format Used | Directory in Container               | Purpose                                                                                         |
| ------------------ | ----------- | ------------------------------------ | ----------------------------------------------------------------------------------------------- |
| **haproxy.cfg**    | XML         | `/usr/local/etc/haproxy/haproxy.cfg` | The haproxy.cfg file defines some global configuration options that apply to all or many cores. |

## Template variables

| Variable                 | Description                    | Type   | Example |
| ------------------------ | ------------------------------ | ------ | ------- |
| **hostport-port-number** | HAproxy port.                  | int    | 8080    |
| **host-port:**           | HAproxy nodeport               | string | 8080    |
| **haproxy-image-tag**    | HAproxy image version.         | string | 2.7.1   |
| **backend-hostname**     | backend runnable path.         | string | 1.1.1.1 |
| **backend-port**         | backend runnable service name. | string | 443     |

## Local Deployment

| First clone the repository and simply run below command after launching `monkd`: |
| :------------------------------------------------------------------------------: |

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 haproxy/haproxy
 ├─🔗 Process groups:
 │  └─🧩 haproxy/stack
 └─⚙️ Entity instances:
    └─🧩 haproxy/haproxy/metadata
✔ All templates loaded successfully

➜  monk list haproxy

✔ Got the list
Type      Template         Repository  Version  Tags
runnable  haproxy/haproxy  local       -        self hosted, Load balancer
group     haproxy/stack    local       -        -



➜  monk run haproxy/stack

✔ Started local/haproxy/stack

```

This will start the entire haproxy/stack.

## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name haproxy
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add -p aws -f <path/to/your-credentials-file>
✔ Provider added successfully

➜  monk cluster grow -p  aws
? Name of the new instance haproxy-instance
? Tags (split by whitespace) haproxy
? Region eu-north-1
? Instance type t3.large
? Number of instances (or press ENTER to use default = 1) 3
? Default disk type for aws is HDD (standard). Would you like to change it? Yes
? Disk type SSD (gp3)
? Disk size (or press ENTER to use default = 250 GBs) 50
✔ Start creation of new instance(s) on aws... DONE
✔ Creating node: haproxy-instance-1 DONE
✔ Initializing node: haproxy-instance-1 DONE
✔ Creating node: haproxy-instance-2 DONE
✔ Initializing node: haproxy-instance-2 DONE
✔ Creating node: haproxy-instance-3 DONE
✔ Initializing node: haproxy-instance-3 DONE
✔ Connecting: haproxy-instance-1 DONE
✔ Syncing peer: haproxy-instance-1 DONE
✔ Connecting: haproxy-instance-2 DONE
✔ Syncing peer: haproxy-instance-2 DONE
✔ Connecting: haproxy-instance-3 DONE
✔ Syncing peer: haproxy-instance-3 DONE
✔ Syncing nodes DONE
✔ Cluster grown successfully
```

Once cluster is ready execute the same command as for local and select your cluster (the option will appear automatically).

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 haproxy/haproxy
 ├─🔗 Process groups:
 │  └─🧩 haproxy/stack
 └─⚙️ Entity instances:
    └─🧩 haproxy/haproxy/metadata
✔ All templates loaded successfully

➜  monk list haproxy

✔ Got the list
Type      Template         Repository  Version  Tags
runnable  haproxy/haproxy  local       -        self hosted, Load balancer
group     haproxy/stack    local       -        -



➜  monk run haproxy/stack

✔ Started local/haproxy/stack

```

## Logs & Shell

```bash
# show HAproxy logs
➜  monk logs -l 1000 -f haproxy/haproxy

# access shell in the container running HAproxy
➜  monk shell haproxy/haproxy

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge  --ii --rv --rs --no-confirm --rv --rs haproxy/haproxy haproxy/stack

✔ haproxy/haproxy purged
✔ haproxy/haproxy purged
```
