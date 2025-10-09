# Hacking Labs

Welcome to the lab setup instructions for the lecture **Ethical Hacking**. In this guide you will learn how to configure and access the Docker-based lab environment we’ll be using throughout the course. Follow these steps carefully to ensure proper functionality.

---

### Part 1: What are Docker containers and Docker Compose files

**Docker container**: a lightweight, standalone environment that packages an application and everything it needs to run (OS libraries, binaries, dependencies). Containers are isolated from your host but can be configured to share networks and ports.

**Docker image**: a read-only template used to create containers. Images are stored on registries (like Docker Hub) and pulled to your machine when you start containers.

**docker-compose file** (`.yml`): a YAML file that defines one or more services (containers), networks, volumes and how they connect. `docker-compose` reads these files and creates the whole multi-container application (networks + volumes + containers) with a single command. 
All docker-compose files will be provided on the following [Github repository](https://github.com/Devrim-Celik/hacking-labs).****

Why we use them in this course:
- Each lab is modeled as a set of containers (target machines + helper machines). Using `docker-compose` makes it easy to bring the entire lab up and down consistently.

---

### Part 2: How to manage a docker-compose file

Below are the common operations you will use every lab session. Replace `file.yml` with the name(s) of your compose files.

**Start (bring services up)**
Start all services defined in a compose file in detached (background) mode:
```bash
docker-compose -f file.yml up -d
```

Start two files together:
```bash
docker-compose -f file01.yml -f file02.yml up -d
```

**Stop** (stops containers but keeps them on disk)
```bash
docker-compose -f file.yml stop
```
`stop` gracefully stops the running containers (like powering them off). The containers remain on your system and can be started again.

**Start (after stop)**
If you previously ran `stop`, bring them back:
```bash
docker-compose -f file.yml start
```

**Restart** (stop then start a container)
```bash
docker-compose -f file.yml restart
```

**Down** (stop and remove containers, networks and default volumes created by the compose)
```bash
docker-compose -f file.yml down
```
`down` is the safe way to fully tear down a lab session. It removes the containers and networks that `up` created (by default it does not remove externally defined volumes unless you pass `-v`).

**Remove only stopped containers**
If you want to remove stopped containers for the compose project only:
```bash
docker-compose -f file.yml rm
```

**Update images**
When new Docker images are released, `docker-compose` does not automatically pull the newest image when you run `up`. Pull images explicitly:
```bash
docker-compose -f file.yml pull
```
After pulling, recreate containers so they use the updated images:
```bash
docker-compose -f file.yml up -d --force-recreate
```
If you only want to recreate one service (for example `attacker-system`) without its dependencies:
```bash
docker-compose -f base.yml up -d --no-deps --force-recreate attacker-system
```

**Useful flags**
- `-d` — run in detached/background mode.
- `--force-recreate` — recreate containers even if the configuration hasn’t changed.
- `--no-deps` — when recreating a single service, don’t start linked dependency services.

**Quick checks**

- See containers and open ports: `docker ps`
- Inspect logs: `docker logs -f <container-name>`

---

### Part 3: `base.yml`
Can be started using:
    ```bash
    docker-compose -f base.yml up -d
    ```
`base.yml` contains two persistent machines used in nearly every session:

1. **lab-assistant**
    - Purpose: menu-driven helper that contains exercise descriptions, objectives and navigation to all labs and their exercises. Use this container to read what you should do next for a lab.
    - SSH port on host: **10022**
    - Credentials: `student` / `student`
    - Connect:  
    ```bash
    ssh student@localhost -p 10022
    ```
    - What you’ll find: a simple menu listing labs and exercises with descriptions, objectives and hints.
2. **attacker-system**
    - Purpose: Kali-based attacker machine with tools preinstalled. This is the primary interface to interact with the lab network and run scans, exploits, pivoting, etc.
    - SSH port on host: **10023**
    - Credentials: `attacker` / `attacker`
    - Connect:  
    ```bash
    ssh attacker@localhost -p 10023
    ```
    - Notes: the attacker-system is attached to the same Docker network as the targets and can reach them by container name or IP. It is the system  to run network tools against target containers.


---

### Part 4: `ethical-hacking_lab0X.yml`

Each lab file (named like `ethical-hacking_lab01.yml`, `ethical-hacking_lab02.yml`, etc.) defines the _target machines_ and services you will attack or analyze in that lab.

**Start a lab together with base.yml (recommended)**
From the directory containing both files run:
```bash
docker-compose -f base.yml -f ethical-hacking_lab0X.yml up -d
```

This command merges the two files so that the persistent services in `base.yml` and the targets in the lab file are created on a common Docker network and can communicate.

**How to interact with targets**
- **Use `lab-assistant`** to read the exercise description and objectives for the lab and specific tasks.
- **Use `attacker-system`** to run your tools  against the target containers. The attacker-system is already on the lab’s Docker network and can reach targets by hostname or internal IP.
- **When a target is a web server that maps ports to the host**, you can access it directly from your host machine via `http://localhost:<mapped-port>`.

**Stop / tear down when finished**

When you finished the lab and want to clean up everything the combined compose created:
```bash
docker-compose -f base.yml -f ethical-hacking_lab0X.yml down
```
This removes containers and networks created by the `up` command. If you want to also remove associated volumes, add `-v`.

---

## One-line Cheat-sheet
Start lab: `docker-compose -f base.yml -f ethical-hacking_lab0X.yml up -d`
SSH lab-assistant: `ssh student@localhost -p 10022`
SSH attacker-system: `ssh attacker@localhost -p 10023`
Stop: `docker-compose -f base.yml -f ethical-hacking_lab0X.yml stop`
Down: `docker-compose -f base.yml -f ethical-hacking_lab0X.yml down`
Pull updates: `docker-compose -f base.yml -f ethical-hacking_lab0X.yml pull`

