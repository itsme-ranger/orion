# Orion — Build & Run with Docker

This guide explains how to **build** and **run** the Orion Docker image with a host-mounted configuration directory and a custom bootstrap YAML file.

---

## 1. Build the Docker Image

From the root of the `orion` project:

```bash
cd orion
docker build -t orion -f docker/Dockerfile .
```

This will create a Docker image named `orion`.

---

## 2. Prepare Configuration

- Create or locate your **bootstrap configuration YAML** (e.g., `bootstrap.yaml`)
- Place it inside your host directory:  
  ```
  orion/orion-proxy/conf
  ```

Example structure:

```
orion/
  orion-proxy/
    conf/
      bootstrap.yaml
```

---

## 3. Run the Container

```bash
docker run --rm     --name orion     -v $(pwd)/orion/orion-proxy/conf:/orion/orion-proxy/conf     orion     --config /orion/orion-proxy/conf/<bootstrap.yaml>
```

Replace `<bootstrap.yaml>` with the actual filename of your bootstrap config.

---

### Arguments Explained

- `--rm` — Removes the container after it exits  
- `--name orion` — Names the container `orion`  
- `-v $(pwd)/orion/orion-proxy/conf:/orion/orion-proxy/conf` — Mounts the host config directory into the container  
- `orion` — The Docker image name (replace with your actual image tag if needed, e.g., `orion:latest`)  
- `--config` — Path to your bootstrap YAML file inside the container

---

## 4. Example Run

If your bootstrap file is `demo-bootstrap.yaml` inside `orion/orion-proxy/conf`:

```bash
docker run --rm     --name orion     -v $(pwd)/orion/orion-proxy/conf:/orion/orion-proxy/conf     orion     --config /orion/orion-proxy/conf/demo-bootstrap.yaml
```

---

## Notes

- The `--config` path **must** point to a file inside `/orion/orion-proxy/conf` in the container.  
- Ensure your YAML file is valid and matches Orion’s configuration schema.  
- If you need logs or persistent data, consider mapping additional volumes.
