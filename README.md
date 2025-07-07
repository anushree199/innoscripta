Here is the full `README.md` content without any special symbols, ready for clean copy-paste:

---

# OpenStack Deployment Using Ansible on Ubuntu 22.04

## Project Objective

This project automates the deployment of an OpenStack environment (DevStack-based) on Ubuntu 22.04 using Ansible. It deploys core OpenStack services and configures networking, security, and validation steps.

### Goals:

* Deploy core OpenStack services:

  * Compute (Nova)
  * Storage (Swift)
  * Database (Trove)
* Automate public/private networking with Neutron
* Configure keypair authentication and RBAC
* Enable monitoring using Ceilometer
* Simulate scalability and HA behavior
* Provide rollback-safe and idempotent deployment
* Include validation steps for successful setup

---

## Folder Structure

```
openstack-ansible/
├── inventory/
│   └── hosts.yml
├── group_vars/
│   └── all.yml
├── files/
│   └── local.conf
├── playbooks/
│   ├── site.yml
│   ├── security.yml
│   ├── network.yml
│   ├── validate.yml
├── roles/
│   └── devstack/
│       └── tasks/
│           └── main.yml
│       └── handlers/
│           └── main.yml
├── README.md
```

---

## How to Run

### Step 1: Deploy OpenStack with DevStack

```bash
ansible-playbook -i inventory/hosts.yml playbooks/site.yml
```

### Step 2: Apply Security Configuration (SSH Keypair + RBAC)

```bash
ansible-playbook -i inventory/hosts.yml playbooks/security.yml
```

### Step 3: Set Up Networking (Public/Private + Router)

```bash
ansible-playbook -i inventory/hosts.yml playbooks/network.yml
```

### Step 4: Validate the Deployment

```bash
ansible-playbook -i inventory/hosts.yml playbooks/validate.yml
```

---

## What This Deploys

| Component  | Status | Description                                        |
| ---------- | ------ | -------------------------------------------------- |
| Nova       | Yes    | Compute service to launch virtual machines         |
| Swift      | Yes    | Object storage for file upload/download            |
| Trove      | Yes    | Database-as-a-service for PostgreSQL, MySQL, etc.  |
| Keystone   | Yes    | Identity management and authentication             |
| Neutron    | Yes    | Public/private network, router, and security rules |
| Ceilometer | Yes    | Monitoring and telemetry service                   |
| Keypair    | Yes    | Used to SSH into virtual machines                  |
| RBAC       | Yes    | Role-based access control assigned to demo user    |

---

## Validation Checklist

### 1. Check all OpenStack services

```bash
openstack service list
```

### 2. Create and list compute instances

```bash
openstack server create --image cirros --flavor m1.tiny --network private --key-name devstack-key test-vm
openstack server list
```

### 3. Assign floating IP and SSH

```bash
openstack floating ip create public
openstack server add floating ip test-vm <your-fip>
ssh -i ~/.ssh/id_rsa cirros@<your-fip>
```

### 4. Upload and download object from Swift

```bash
openstack container create mycontainer
openstack object create mycontainer sample.txt
```

### 5. Create a Trove DB instance

```bash
openstack database instance create --flavor m1.small --volume 5 --databases testdb --users user1:password db1
```

---

## Security Features

* SSH keypair created (`devstack-key`)
* Default security group allows TCP port 22 (SSH)
* Admin role granted to demo user for RBAC

---

## Monitoring and Logging

* Ceilometer is enabled for telemetry
* Logs available in `/opt/stack/logs` (local only)

---

## Simulated Scaling and HA

| Feature           | Implemented  | Notes                                 |
| ----------------- | ------------ | ------------------------------------- |
| Nova Auto-Scaling | Simulated    | Requires Heat templates               |
| Swift Scaling     | Simulated    | Manual ring rebalance can be scripted |
| Multi-Node HA     | Not included | Needs multiple VMs or cloud setup     |

---

## Known Limitations

* HA is simulated on a single node, not fully distributed
* Swift encryption and scaling are documented but not fully implemented
* Heat-based auto-scaling is optional and not enabled by default
* Centralized logging is not set up

---

## Troubleshooting Tips

* If `stack.sh` fails, clean `/opt/devstack` and run again
* If CLI fails, run `source /opt/devstack/openrc admin`
* If SSH to VM fails, check security groups and floating IP
* Use `-vvv` in Ansible to see detailed debug output

---

This project provides a fully automated Ansible deployment for OpenStack with Nova, Swift, Trove, Neutron, and Ceilometer. It satisfies core requirements and simulates advanced features where infrastructure limits apply. Code is idempotent and modular. Full deployment and validation instructions are included.

Sorry, I was unable to take screenshots and make presentation.


