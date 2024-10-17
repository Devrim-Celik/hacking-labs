# Hacking Labs

### Part 1: Setting Up the Docker Containers

Each lab will be provided as a `.yml` file through this repository. These `.yml` files are designed for **docker-compose**, specifying the necessary Docker images, network configuration, and ports that need to be exposed on your host system. The Docker images specified in the `.yml` file are published on my Dockerhub repositories.

**Steps to set up the Docker environment:**
1. **Run Docker Compose** after downloading the `.yml` file with the following command to download the required Docker images from DockerHub and set up the environment. Through the use of the `-d` flag (*detach*), the images will run in the background.

   ```
   docker-compose -f <lab-file>.yml up -d
   ```

   Replace `<lab-file>` with the actual name of the `.yml` file provided for the specific lab.

2. **Managing Docker containers**:
   - To stop (as in pause) the Docker environment:
   ```
   docker-compose -f <lab-file>.yml stop
   ```
   - To start the Docker environment after stopping:
   ```
   docker-compose -f <lab-file>.yml stop
   ```
   - To stop and remove the Docker environment from your system:
   ```
   docker-compose -f <lab-file>.yml down
   ```

### Part 2: Accessing the Attacker System

For each lab, you will be interacting with an `attacker_system` that runs a Docker image based on **Kali Linux**. This system has all the necessary tools pre-installed, along with an SSH server for remote access. For most labs, this image will serve as your gateway into the built Docker environment.

To access the `attacker_system` after setting up the Docker environment:

1. **Connect to the attacker system via SSH**:

   The SSH server of the `attacker_system` will be attached to port `10022` of your host system. Use the following command to connect to it:

   ```
   ssh attacker@localhost -p 10022
   ```
   Credentials: `attacker:attackerpassword`

2. **Using the `lab-assistant` utility**:
   To streamline the process and eliminate the need for separate instruction documents, I've created a Whiptail-based command-line utility on the `attacker_system` that provides all the necessary information for each lab. Once you’re connected to the attacker system, you can use the `lab-assistant` tool to access detailed lab instructions. This utility will guide you through the objectives and tasks, ensuring you have all the information you need directly within the lab environment.

   Simply run:
   ```
   lab-assistant
   ```
   You can navigate the `lab-assistant` using the **Enter**, **Tab** and **Arrow** keys. 

---

### Recap

- **Set up Docker containers** using the `.yml` file provided.
- **Connect to the attacker system** via SSH (port `10022`) with the given credentials.
- Use the **`lab-assistant` command** to get lab instructions.

Make sure you follow these steps carefully for each lab session. If you encounter any issues, feel free to ask for assistance!

As a final note, I encourage you to take note of anything in the labs that you feel could be better explained, implemented, or configured. Your feedback is invaluable in helping me improve these labs, and I welcome your suggestions on how to enhance the overall experience. Please don't hesitate to share your thoughts with me. Thank you in advance and have fun with the labs!

### Disclaimer
By accessing and utilizing these ethical hacking labs, you acknowledge and agree to the following terms:
1. Intended Use: These labs are designed solely for educational purposes to enhance your understanding of ethical hacking, cybersecurity concepts, and related practices. You agree to use the labs in a responsible manner that complies with all applicable laws and ethical standards.

2. Local Environment: All lab exercises and configurations are intended to be executed on your local machine via Docker containers. These images are vulnerable by design, meant to provide a safe environment for learning. While the Docker configuration files here specify that all access to the images should be attacked to only the local interface, make sure to never expose them on any external network. You are responsible for ensuring that your environment is secure and not inadvertently accessible to others.

3. Liability: The author of these labs and related materials shall not be held liable for any damages or consequences arising from the use or misuse of the labs. You use these resources at your own risk.

By proceeding with these labs, you confirm that you have read, understood, and agreed to this disclaimer. Thank you for your commitment to ethical practices in cybersecurity education.

