## 🌐 [Versão em Português do README](README.md)

# Docker Volume Backup and Restore

This is a utility for backing up and restoring Docker volumes. It works just as well with Podman volumes, simply replace the `docker` command with `podman`.

> **Warning**: Make sure no container is using the volume before performing backup or restore, otherwise your data might get corrupted. See "Miscellaneous" for more details.

> **Note**: When using `docker-compose`, make sure to backup and restore volume labels. See "Miscellaneous" for more information.

## 🔨 Project Features

- Backup Docker volumes with support for different compression algorithms.
- Restore volumes from backup files with the option to force overwrite existing data.
- Exclude files or directories during the backup process.
- Migrate volumes between hosts over SSH.
- Optionally preserve volume labels when needed (important for `docker-compose`).

### Visual Example of the Project

**Backup a volume:**

```bash
docker run -v some_volume:/volume --rm --log-driver none loomchild/volume-backup backup > some_archive.tar.bz2
```

**Restore a volume:**

```bash
docker run -i -v some_volume:/volume --rm loomchild/volume-backup restore < some_archive.tar.bz2
```

## ✔️ Techniques and Technologies Used

- **Docker**: To create, manage, and manipulate Docker volumes.
- **Tar**: Used for archiving and extracting data during the backup and restore process.
- **Compression Algorithms**: Support for `bz2`, `gz`, `xz`, `pigz`, `zstd`, and no compression.

## 📁 Project Structure

- **Dockerfile**: File to build the Docker image.
- **LICENSE**: License of the project.
- **README.md**: Project documentation.
- **volume-backup.sh**: Main script for backing up and restoring Docker volumes.

## 🛠️ Running the Project Locally

To use this utility locally, follow the steps below:

1. **Make sure Docker is installed**:
    - [Docker](https://www.docker.com/get-started) is required to run the project. You can check if it is installed by running:

      ```bash
      docker -v
      ```

    - If Docker is not installed, download and install the recommended version.

2. **Clone the Repository**:
    - Copy the repository URL and run the following command in your terminal:

      ```bash
      git clone https://github.com/username/volume-backup.git
      cd volume-backup
      ```

3. **Build the Docker Image**:
    - Run the following command to build the Docker image:

      ```bash
      docker build -t volume-backup .
      ```

4. **Run Backup or Restore**:
    - **Backup**:

      ```bash
      docker run -v [volume-name]:/volume --rm --log-driver none volume-backup backup > [archive-path]
      ```

    - **Restore**:

      ```bash
      docker run -i -v [volume-name]:/volume --rm --log-driver none volume-backup restore < [archive-path]
      ```

## 🌐 Deploying the Project

To deploy the project in production or on another environment, follow the steps below:

1. **Push the Docker image to DockerHub or GitHub Container Registry**:
    - You can push the image to DockerHub or use GitHub Container Registry to avoid DockerHub usage limits.

      ```bash
      docker tag volume-backup ghcr.io/username/volume-backup
      docker push ghcr.io/username/volume-backup
      ```

2. **Run Backup or Restore on the Production Host**:
    - On the production host, execute the same backup and restore commands as described above.

## Miscellaneous

- **Update the image**:

  ```bash
  docker pull loomchild/volume-backup
  ```

  Or use GitHub Container Registry to avoid DockerHub usage limits:

  ```bash
  docker pull ghcr.io/loomchild/volume-backup
  ```

- **Find containers using a volume**:

  ```bash
  docker ps -a --filter volume=[volume-name]
  ```

- **Exclude files from the backup**:

  ```bash
  docker run -v [volume-name]:/volume --rm --log-driver none loomchild/volume-backup backup -e [excluded-glob] > [archive-path]
  ```

- **Use a different compression algorithm**:

  ```bash
  docker run -v [volume-name]:/volume --rm --log-driver none loomchild/volume-backup backup -c pigz > [archive-path]
  ```

- **Show progress indicator**:

  ```bash
  docker run -v [volume-name]:/volume --rm --log-driver none loomchild/volume-backup backup -v > [archive-path]
  ```

- **Pass additional arguments to the `tar` utility**:

  ```bash
  docker run -v [volume-name]:/volume --rm --log-driver none loomchild/volume-backup backup -x --verbose > [archive-path]
  ```

- **Migrate volume directly to a new host via SSH**:

  ```bash
  docker run -v [volume-name]:/volume --rm --log-driver none loomchild/volume-backup backup | ssh [receiver] docker run -i -v [volume-name]:/volume --rm loomchild/volume-backup restore
  ```

- **Preserve volume labels**: If you are using `docker-compose`, save and restore volume labels with:

  ```bash
  docker inspect [volume-name] -f "{{json .Labels}}" > labels.json
  ```

  During the restore, manually create the volume with the labels:

  ```bash
  docker volume create --label "label1" --label "label2" [volume-name]
  ```

```

