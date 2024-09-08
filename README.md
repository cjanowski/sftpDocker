# SFTP Server Dockerfile

This project provides a Dockerfile to build a simple SFTP server using Ubuntu 20.04 and OpenSSH. It's designed for easy setup and local hosting of an SFTP server.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Building the Docker Image](#building-the-docker-image)
- [Running the SFTP Server](#running-the-sftp-server)
- [Connecting to the SFTP Server](#connecting-to-the-sftp-server)
- [Security Considerations](#security-considerations)
- [Customization](#customization)

## Prerequisites

- Docker installed on your system
- Basic understanding of Docker and SFTP

## Building the Docker Image

1. Save the Dockerfile in your project directory.
2. Open a terminal and navigate to the directory containing the Dockerfile.
3. Build the Docker image with the following command:

```bash
docker build -t sftp-server .
```

This command builds the Docker image and tags it as `sftp-server`.

## Running the SFTP Server

To run the SFTP server:

```bash
docker run -d -p 2222:22 -v /path/to/sftp/data:/sftp sftp-server
```

Replace `/path/to/sftp/data` with the path to the directory on your host machine where you want to store the SFTP data.

## Connecting to the SFTP Server

Before connecting, you need to add an SSH public key for the `sftpuser`:

1. Copy your public key to the container:

```bash
docker cp /path/to/your/publickey.pub container_id:/home/sftpuser/.ssh/authorized_keys
```

2. Set the correct permissions:

```bash
docker exec container_id chown sftpuser:sftpuser /home/sftpuser/.ssh/authorized_keys
```

Replace `container_id` with your actual container ID, and `/path/to/your/publickey.pub` with the path to your SSH public key.

To connect to the SFTP server:

```bash
sftp -P 2222 sftpuser@localhost
```

## Security Considerations

- This setup uses key-based authentication and disables password authentication for improved security.
- The SFTP user is chrooted to the `/sftp` directory for isolation.
- For production use, consider additional security measures such as firewall rules and regular security updates.

## Customization

You can customize the Dockerfile to:
- Use a different base image
- Add multiple SFTP users
- Change the SFTP root directory
- Add additional packages or configurations

Remember to rebuild the Docker image after making changes to the Dockerfile.

---

For more detailed information about the Dockerfile and its contents, please refer to the comments within the Dockerfile itself.
