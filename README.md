# Hello World GKE

A simple project to quickstart GKE


## How to run this app on GKE?
### Prerequistes
- Ensure you have [GCP](https://cloud.google.com/) account.
- Enable the Cloud Build and Google Kubernetes Engine APIs.
- Google Cloud SDK (`gcloud` command) is installed and initialized. You can install and initialize it by following [the official installation guide](https://cloud.google.com/sdk/docs/install).
- `kubectl` command is installed. You can install it by using `gcloud` command:

  ```sh
  gcloud components install kubectl
  ```

### Containerizing an app with Cloud Build
This app is containerized with [`Dockerfile`](./Dockerfile).
Build your container image running the following command:

```sh
gcloud builds submit --tag gcr.io/$(gcloud config get-value project)/helloworld-gke .
```

Your image is build on Google Cloud by using [Cloud Build](https://cloud.google.com/build), and stored in [Container Registry](https://cloud.google.com/container-registry).

### Creating a GKE cluster
A GKE cluster is a managed set of Compute Engine virtual machines that operate as a single GKE cluster.  
You can create the cluster with the following command:

```sh
# Replace `COMPUTE_ZONE` with the Google Cloud zone where you want to host your cluster, such as `us-west1-a`
gcloud container clusters create helloworld-gke \
    --num-nodes 1 \
    --zone COMPUTE_ZONE
```

You can verify that you have access to the cluster with the following command. The command lists the nodes in your container cluster which are up and running and indicates that you have access to the cluster.

```sh
kubectl get nodes
```

### Deploying to GKE
To deploy your app to the GKE cluster you created, you need two Kubernetes objects.

1. A Deployment to define your app.
1. A Service to define how to access your app.

#### Deploy an app
Run the following command. Before running the command, you need to replace $GCLOUD_PROJECT in the `deployment.yml` file with your project ID.

```sh
kubectl apply -f deployment.yaml
```

#### Deploy a Service
1. Create the Hello World Service:

```sh
kubectl apply -f service.yaml
```

1. Get the external IP address of the Service:

```sh
kubectl get services
```

It can take up to 60 seconds to allocate the IP address. The external IP address is listed under the column EXTERNAL-IP for the hello Service.

#### View a deployed app
You have now deployed all the resources needed to run the Hello World app on GKE.

Use the external IP address from the previous step to load the app in your web browser, and see your running app:

```
 http://EXTERNAL_IP
 ```

 Or, you can make a curl call to the external IP address of the Service:

```sh
curl EXTERNAL_IP
```

The output displays the following:

```
Hello World!
```

Congrats! 🎉 Your app is running on GKE!


## Refference
- [Quickstart guide of GKE](https://cloud.google.com/kubernetes-engine/docs/quickstarts/deploying-a-language-specific-app#ruby)
