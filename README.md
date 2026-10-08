# networking-lab-4hosts-2subnets

Networking lab with 4 hosts, 2 subnets, and a Linux router.

## Project Overview

This project implements a small routed network using Containerlab.

The topology consists of four hosts divided into two separate IPv4 subnets. Each subnet contains two hosts and is connected to a router through a Layer 2 bridge implemented using an Alpine Linux container.

The router provides connectivity between the two subnets and has IP forwarding enabled.

### Network Architecture

* **Subnet A:** `192.168.10.0/24`

  * `host-a1`: `192.168.10.2/24`
  * `host-a2`: `192.168.10.3/24`
  * Gateway: `192.168.10.1`

* **Subnet B:** `192.168.20.0/24`

  * `host-b1`: `192.168.20.2/24`
  * `host-b2`: `192.168.20.3/24`
  * Gateway: `192.168.20.1`

The router uses:

* `eth1`: `192.168.10.1/24`
* `eth2`: `192.168.20.1/24`

IP forwarding is enabled on the router to allow communication between the two subnets.

## Topology Diagram

```text
                       Router
              ┌─────────────────────┐
              │                     │
              │ eth1: 192.168.10.1  │
              │ eth2: 192.168.20.1  │
              │                     │
              └─────────┬───────────┘
                        │
              ┌─────────┴─────────┐
              │                   │
           Switch A            Switch B
              │                   │
        ┌─────┴─────┐       ┌─────┴─────┐
        │           │       │           │
      host-a1     host-a2  host-b1     host-b2
     .10.2/24    .10.3/24 .20.2/24    .20.3/24

        192.168.10.0/24      192.168.20.0/24
```

## Verification Instructions

### 1. Validate the YAML Syntax Using Python

Python can be used to check whether the `topology.yaml` file contains valid YAML syntax.

First, install PyYAML if necessary:

```bash
pip install pyyaml
```

Then run:

```bash
python -c "import yaml; yaml.safe_load(open('topology.yaml')); print('YAML syntax is valid')"
```

If the file contains valid YAML syntax, the command will output:

```text
YAML syntax is valid
```

If there is a syntax error, Python will report the location of the error.

### 2. Validate the Containerlab Topology

If Containerlab is installed, run:

```bash
containerlab deploy --topo topology.yaml
```

Containerlab will parse the topology and report any configuration errors.

### 3. Verify Connectivity

After deploying the topology, connectivity can be tested from `host-a1`:

```bash
ping 192.168.10.3
ping 192.168.20.2
ping 192.168.20.3
```

The first test verifies communication within Subnet A.

The second and third tests verify routing from Subnet A to Subnet B through the router.

## Files

* `topology.yaml` - Containerlab topology definition.
* `README.md` - Project documentation and verification instructions.
