# 🦊 Anime Recommender System
The Anime Recommender App intelligently suggests the top 3 anime titles based on genre preferences extracted from user queries. 
It leverages the power of Large Language Models (LLMs) through the GROQ inference API, orchestrated using the LangChain framework.

## 🔧 Core Workflow
- **🗣️ User Query:** Accepts natural language input describing anime preferences.
- **🧠 LLM Processing:** GROQ-powered LLM interprets the query and extracts relevant genres.
- **📊 Dataset Matching:** Anime metadata from an Excel dataset is used as the source for recommendations.
- **🔍 Semantic Embedding:** The dataset is converted into vector embeddings using Hugging Face’s Sentence Transformers.
- **🗃️ Vector Search:** Embeddings are stored locally in a FAISS vector database for fast similarity search.
- **🖥️ Frontend Interface:** A Streamlit app provides an interactive UI for users to input queries and view results.
- **🐳 Deployment:** The entire application is containerized with Docker and deployed on a Kubernetes cluster running in a GCP VM instance.

## 📦 Installating & Running Locally
- **Clone this repo & CD Anime-Recommender-System-LLMOPS :**
  ```
  git clone https://github.com/P-RajaRamesh/Anime-Recommender-System-LLMOPS.git
  ```
- **Create Virtual Environment :** ```conda create -p venv1 python==3.10```
- **Activate Virtual Environment :** ```conda activate venv1\```
- **Install Requirements :** ```pip install -e .```
- **Create ```.env``` file:**
  ```
  GROQ_API_KEY="<YOUR-GROQ-API-KEY>"
  HUGGINGFACEHUB_API_TOKEN="<YOUR-HUGGINGFACEHUB-API-TOKEN>"
  ```
- Build the pipeline to push the anime dataset to FAISS vector store: ```pipeline/build_pipeline.py```
- Finally Run Streamlit app: ```streamlit run app/app.py```

## 💻 Pushing code to your Github
```
git init
git add .
git commit -m "commit"
git branch -m main
git remote add origin https://github.com/P-RajaRamesh/Anime-Recommender-System-LLMOPS.git
git push origin main
```

## ☁️ Deploying in Google CLoud VM:
### 1. Goto VM Instances and click **"Create Instance"**
  - Name: `Anime-Recommender-System`
  - Machine Type:
    - Series: `E2`
    - Preset: `Standard`
    - Memory: `16 GB RAM`
  - Boot Disk:
    - Change size to `150 GB`
    - Image: Select **Ubuntu 24.04 LTS**
  - Networking:
    - Enable HTTP and HTTPS traffic
  - **Create the Instance**
---

### 2. Connect to the VM
  - Use the **SSH** option provided to connect to the VM from the browser.
---

### 3. Install Docker
  - Search: "Install Docker on Ubuntu"
  - Open the first official Docker website (docs.docker.com) - https://docs.docker.com/engine/install/ubuntu/
  - Scroll down and copy the **first big command block** and paste into your VM terminal : 1. Set up Docker's apt repository.
     ```
      # Add Docker's official GPG key:
      sudo apt-get update
      sudo apt-get install ca-certificates curl
      sudo install -m 0755 -d /etc/apt/keyrings
      sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
      sudo chmod a+r /etc/apt/keyrings/docker.asc
      
      # Add the repository to Apt sources:
      echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
        $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      sudo apt-get update
     ```
  - Then copy and paste the **second command block** : 2. Install the Docker packages.
    ```
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```
  - Then run the **third command** to test Docker: 3. You will get error, you need to use sudo before it

    ```bash
    docker run hello-world
    ```

  - **Run Docker without sudo**
    - On the same page, scroll to: **"Post-installation steps for Linux"** : https://docs.docker.com/engine/install/linux-postinstall/
    - Paste all 4 commands one by one to allow Docker without `sudo`
      1. Create the docker group: ```sudo groupadd docker```      
      2. Add your user to the docker group: ```sudo usermod -aG docker $USER``` 
         - Log out and log back in so that your group membership is re-evaluated.
      If you're running Linux in a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.
      3. You can also run the following command to activate the changes to groups: ```newgrp docker```    
      4. Verify that you can run docker commands without sudo: ```docker run hello-world```

  - **Enable Docker to start on boot**
    - On the same page, scroll down to: **"Configure Docker to start on boot"**
    - Copy and paste the command block (2 commands one-by-one):
  
      ```bash
      sudo systemctl enable docker.service
      sudo systemctl enable containerd.service
      ```

  - **Verify Docker Setup** : ```docker version```
---

### 4. Install Minikube
  - Open browser and search `Install Minikube` 
  - Open the first official site (minikube.sigs.k8s.io) with `minikube start` on it: : https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download
  - Choose:
    - **OS:** Linux
    - **Architecture:** *x86*
    - Select **Binary download**
    - To install the latest minikube stable release on x86-64 Linux using binary download:
      1. Copy and run the below commands one-by-one
      ```
      curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
      sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
      ```
      2. Start your cluster: ```minikube start```
      - This uses Docker internally, which is why Docker was installed first
---

### 5. Install kubectl
  - Search: `Install kubectl on Ubuntu` : https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
  - Run the first command with `curl` from the official Kubernetes docs: 1. Download the latest release with the command:
    ```
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    ```
  - Go to the **Snap section** (below on the same page) : ```sudo snap install kubectl --classic```
  - Verify installation: ```kubectl version --client```
---

### 6. Interlink your Github to VM
  - **Clone this repo & CD Anime-Recommender-System-LLMOPS :**
    ```
    git clone https://github.com/P-RajaRamesh/Anime-Recommender-System-LLMOPS.git
    cd Anime-Recommender-System-LLMOPS
    git config --global user.email "rajaramesh7410@gmail.com"
    git config --global user.name "P-RajaRamesh"
    
    git add .
    git commit -m "commit"
    git push origin main
    ```
  - Afetr this it will prompt for :
    - **Username**: `P-RajaRamesh`
    - **Password**: GitHub token (paste, it's invisible)
        - Goto Github Settings -> Developer Settings -> Personal Access Token -> Generate new token (classic)
        - Give the access permissions for the token: admin:org, admin:org_hook, admin:repo_hook, repo, workflow
---

### 7. Create a firewall rule in Google Cloud Console
  - Search for firewall
  - Create a firewall rule
  - Name the firewall : `anime-recommender-firewall`
  - Direction of traffic : Ingress
  - Action on match : Allow
  - Targets : All instances in the network
  - Source filter : IPv4 ranges
  - Source IPv4 ranges : 0.0.0.0/0
  - Protocols and ports : Allow All
---
    
### 8. Build and Deploy your APP on VM
  - Point Docker to Minikube instead locally on VM : ```eval $(minikube docker-env)```
  - Build the Docker image : ```docker build -t llmops-app:latest .```
  - Verify docker image: ```docker images```
  - Configure environment secrets :
    ```
    kubectl create secret generic llmops-secrets \
    --from-literal=GROQ_API_KEY="<YOUR-GROQ-API-KEY>" \
    --from-literal=HUGGINGFACEHUB_API_TOKEN="YOUR-HUGGINGFACEHUB-API-TOKEN"
    ```
  - Verify secrets : ```kubectl get secret llmops-secrets```
  - **Ignore** If wrong secrets entered : ```kubectl delete secret llmops-secrets```
  - **Ignore** If secrets are in another name space :```kubectl delete secret llmops-secrets -n my-namespace```
  - **Ignore** To get name spaces : ```kubectl get ns```
  - Apply Kubernetes deployment file to your cluster : ```kubectl apply -f llmops-k8s.yaml```
  - To see PODs running : ```kubectl get pods```
  - Do minikube tunnel on this terminal for assigning external ip to LoadBalancer : ```minikube tunnel```
  - Open another terminal (SSH), port-forward for quick access externally : ```kubectl port-forward svc/llmops-service 8501:80 --address 0.0.0.0```
---

### 9. Now copy VM External IP in Google Cloud & append ```:8501``` and checkout your Application...
