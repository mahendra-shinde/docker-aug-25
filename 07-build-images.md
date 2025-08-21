# Building Images

Container images can be built using two main methods:

1. **Manual Method** (Using `docker commit`)
2. **Declarative Method** (Using `Dockerfile`)

---

## 1. Manual Method (`docker commit`)

### Benefits
- Quick and easy for small, one-off changes.
- Useful for prototyping or making ad-hoc modifications.
- No need to write a Dockerfile.

### Use-cases
- Experimenting interactively with containers.
- Capturing the state of a running container for debugging.
- Creating images from legacy or hand-crafted environments.

---

## 2. Declarative Method (`Dockerfile`)

### Benefits
- Repeatable and version-controlled builds.
- Easy to share and collaborate.
- Supports automation and CI/CD pipelines.
- Clear documentation of how the image is built.

### Use-cases
- Production-grade image building.
- Automated builds and deployments.
- Open-source projects and team collaboration.

---

## Demo: Manual Method

Let's build a custom image using the manual method.

### Steps

1. **Create a temporary container from `nginx:alpine`:**
   ```sh
   docker run -d --name temp-nginx nginx:alpine
   ```

2. **Get inside the running container:**
   ```sh
   docker exec -it temp-nginx sh
   ```

3. **Replace `index.html` with a new file:**
   ```sh
   echo '<h1>Hello World</h1>' > /usr/share/nginx/html/index.html
   exit
   ```

4. **Capture the changes and create a new image `myapp2`:**
   ```sh
   docker commit temp-nginx myapp2
   ```

5. **Delete the original temporary container:**
   ```sh
   docker rm -f temp-nginx
   ```

6. **Create a new container from the image `myapp2`:**
   ```sh
   docker run -d --name myapp2-container -p 8080:80 myapp2
   ```

7. **Test the home page through browser:**
   - Open [http://localhost:8080](http://localhost:8080) and verify you see "Hello World".

8. **Stop and delete all containers:**
   ```sh
   docker rm -f myapp2-container
   ```

---

## Demo: Declarative Method

Let's build the same image using a `Dockerfile`.

### Steps

1. **Create a new directory and a `Dockerfile`:**
   ```sh
   mkdir myapp
   cd myapp
   ```

2. **Create `index.html`:**
   ```sh
   echo '<h1>Hello World</h1>' > index.html
   ```

3. **Write the `Dockerfile`:**
   ```Dockerfile
   FROM nginx:alpine
   COPY index.html /usr/share/nginx/html/index.html
   ```

4. **Build the image:**
   ```sh
   docker build -t myapp3 .
   ```

5. **Run a container from the new image:**
   ```sh
   docker run -d --name myapp3-container -p 8081:80 myapp3
   ```

6. **Test the home page through browser:**
   - Open [http://localhost:8081](http://localhost:8081) and verify you see "Hello World".

7. **Stop and delete all containers:**
   ```sh
   docker rm -f myapp3-container
   ```

---

## Summary

- **Manual method** is quick for experiments but not scalable.
- **Declarative method** is best for production and collaboration.
- Prefer Dockerfile-based builds for maintainability and automation.
