![Logo](https://www.roomvu.com/_next/image?url=%2Fimages%2Flanding%2Fnew-homepage%2Flogo.svg&w=3840&q=50)


<p align="center"> 
<img src="https://labs.mysql.com/common/logos/mysql-logo.svg" width="100" style="margin-right: 10px;" >
<img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="150">
<img src="https://www.php.net/images/php8/logo_php8_2.svg" width="110" style="margin-right: 10px;"> 
<img src="https://www.nginx.com/wp-content/uploads/2020/05/NGINX-product-icon.svg" width="70" style="margin-right: 10px;"> 
<img src="https://www.docker.com/wp-content/uploads/2022/03/Docker-Logo-White-RGB_Vertical.png" width="100" style="margin-right: 10px;">
<img src="https://kubernetes.io/images/nav_logo2.svg" width="210" style="margin-right: 10px;"> </p>

# DevOps Project  sample task for RoomVu.
# According to the documentation of the previous versions, the final task is presented as requested.


# Task
* Deploy a multi-container application on Kubernetes using Docker. The application consists of three services: a web server, a Laravel base project, and a database. The web server is nginx,and the database is built using MySQL.

* A basic example Laravel project (https://laravel.com/docs/10.x/installation#your-first-laravel-project).

* A Mysql database.
* An Nginx server as a web server.

## Requirements

* Create a Kubernetes cluster using any cloud provider or local environment.
* Create a Dockerfile for the nginx web server that serves as an entry point for applications.
* Create a Dockerfile for the FPM that installs and copies the application code into the container.
* Create a Dockerfile for the database that installs MySQL.
* Use Kubernetes deployment and service objects to deploy the application.
* Use Kubernetes' ingress object to expose the application to the internet.
* Test the deployment by using health check commands and verifying that all services are running correctly and are healthy.
* Run laravel migration after each deployment to update the database structure.

# What has been done :

* **sub task 1:** Create a Kubernetes cluster using any cloud provider or local environment.

In my case, here's a general overview  to create a Kubernetes cluster on my local machine using Minikube or MicroK8s, Because I did not have access to any cloud server or  colocation  Server and I had limited resources i my Core i7 laptop, Anyway, my method went ahead and was tested.

My personal infrastructure build requirements has two main k8s simulation tools and I used them.

**Minikube:**

* I'd Installed Minikube on my local machine.

* Start the Minikube cluster.

* I'd verified the status of the cluster using the command kubectl cluster-info. yes it was up and running.

* I'd deployed applications to the cluster using YAML files. but on real production I suggest to use pre-prepared related project Helm  
  charts. because it's so fast And usable to develop and maintain more professional production.

* Access the deployed applications through their corresponding services. in this case i just used kubernetes dashboard.

**MicroK8s:**

* I'd Installed MicroK8s on your local machine.

* I'd Initialized the MicroK8s cluster using the command microk8s enabled.

* I'd Verify the status of the cluster using the command microk8s kubectl cluster-info.

* I'd deployed applications to the cluster using YAML files.About the helm, I suggest the same as the previous explanation above.

* I'd Accessed the deployed applications through their corresponding services.

* **Please** note that these are just general steps and the specific commands and configurations may vary depending on your operating system and other factors. It's also important to ensure that your machine meets the minimum requirements for running a Kubernetes cluster, such as sufficient CPU and memory resources. I had memory problems. But if you launch in a cloud service or a dedicated server, you will have no problem. Be sure to let me know if you have any installation issues.

# Current structure and test project files:
<pre>
.
├── apache-fpm
│   └── var
│       └── www
│           └── html
├── mysql
│   ├── factories
│   │   └── UserFactory.php
│   ├── migrations
│   │   ├── 2014_10_12_000000_create_users_table.php
│   │   ├── 2014_10_12_100000_create_password_resets_table.php
│   │   ├── 2014_10_12_100000_create_password_reset_tokens_table.php
│   │   ├── 2019_08_19_000000_create_failed_jobs_table.php
│   │   └── 2019_12_14_000001_create_personal_access_tokens_table.php
│   └── seeders
│       └── DatabaseSeeder.php
├── nginx
│   └── nginx.conf
├── README.md
└── roomvu-deployment.yaml

10 directories, 10 files
</pre>

This is a Kubernetes deployment YAML file for an application consisting of three components: nginx, mysql, and laravel.

The nginx component is deployed using the "nginx-deployment" Deployment object and exposed using the "nginx-service" Service object. The "nginx-deployment" object specifies that one replica of the nginx container should be created and started. The "nginx-service" object specifies that the service should use a LoadBalancer type to expose the nginx container on port 80.

The mysql component is deployed using the "mysql-deployment" Deployment object and exposed using the "mysql-service" Service object. The "mysql-deployment" object specifies that one replica of the mysql container should be created and started. The "mysql-service" object specifies that the service should expose the mysql container on port 3306.

The laravel component is deployed using the "laravel-deployment" Deployment object and exposed using the "laravel-service" Service object. The "laravel-deployment" object specifies that one replica of the laravel container should be created and started. The "laravel-service" object specifies that the service should use a LoadBalancer type to expose the laravel container on port 80.

In addition to the containers, there are PersistentVolumeClaim objects defined for both the mysql and laravel deployments, specifying the storage resource requirements for each.

Finally, there is a Secret object called "mysql-secret" which contains the username and password used by the mysql container to authenticate with the mysql database.

Additionally, there is a HorizontalPodAutoscaler object for the nginx deployment, which allows automatic scaling based on CPU usage metrics.


# How To Deployment roomvu-app on kubernetes ? 

* To create and Deployment roomvu-app on kubernetes cluster, you can use **roomvu-deployment.yaml**.

# How to validating configuration files, primarily YAML manifest
# Officela kubernetes schemas, we are using kubeval tool like this:

<pre>
┌──(root㉿DarkHole2)-[/roomvu-tasks/final-task/roomvu-task1-k8s/v3]
└─# kubeval roomvu-deployment2.yaml                    
PASS - roomvu-deployment2.yaml contains a valid Deployment (nginx-deployment)
PASS - roomvu-deployment2.yaml contains a valid Service (nginx-service)
PASS - roomvu-deployment2.yaml contains a valid Deployment (mysql-deployment)
PASS - roomvu-deployment2.yaml contains a valid PersistentVolumeClaim (mysql-pvc)
PASS - roomvu-deployment2.yaml contains a valid Service (mysql-service)
PASS - roomvu-deployment2.yaml contains a valid Deployment (laravel-deployment)
PASS - roomvu-deployment2.yaml contains a valid PersistentVolumeClaim (laravel-pvc)
PASS - roomvu-deployment2.yaml contains a valid Service (laravel-service)
PASS - roomvu-deployment2.yaml contains a valid Secret (mysql-secret)
PASS - roomvu-deployment2.yaml contains a valid HorizontalPodAutoscaler (nginx-hpa)
</pre>

* By running Kubeval on your configuration files, you can perform static analysis and
  validate various aspects such as correct resource types, required fields, format
  compliance, and other rules defined in the Kubernetes schemas. This helps prevent
  issues like invalid configurations, missing values, or incompatible settings that
  could potentially lead to failed deployments or runtime errors.

* I use this validation on minikube and mikrok8s, but if you want to validation on 
  other valiation structure and version you can use somethigns like this, for Examle
  validation for OpenShift - 1.5.1

<pre>
┌──(root㉿DarkHole2)-[/roomvu-tasks/final-task/roomvu-task1-k8s/v3]
└─# kubeval --openshift -v 1.5.1 roomvu-deployment.yaml
PASS - roomvu-deployment.yaml contains a valid Deployment (nginx-deployment)
PASS - roomvu-deployment.yaml contains a valid Service (nginx-service)
PASS - roomvu-deployment.yaml contains a valid Deployment (mysql-deployment)
PASS - roomvu-deployment.yaml contains a valid PersistentVolumeClaim (mysql-pvc)
PASS - roomvu-deployment.yaml contains a valid Service (mysql-service)
PASS - roomvu-deployment.yaml contains a valid Deployment (laravel-deployment)
PASS - roomvu-deployment.yaml contains a valid PersistentVolumeClaim (laravel-pvc)
PASS - roomvu-deployment.yaml contains a valid Service (laravel-service)
PASS - roomvu-deployment.yaml contains a valid Secret (mysql-secret)
PASS - roomvu-deployment.yaml contains a valid HorizontalPodAutoscaler (nginx-hpa)
</pre>

In the given example, it appears to be a command-line output of running the `kubeval` command with certain flags and arguments. `kubeval` is a tool used for validating Kubernetes configuration files against the schema.

Here's a breakdown of the example:

* The current working directory is `/roomvu-tasks/final-task/roomvu-task1-k8s/v3`.
* The command executed is `kubeval --openshift -v 1.5.1 roomvu-deployment.yaml`.
* The `--openshift` flag indicates that the configuration should be validated against the OpenShift schema.
* The `-v 1.5.1` flag specifies the version of Kubernetes schema to use for validation.
* `roomvu-deployment.yaml` is the Kubernetes configuration file being validated.

The output of the command shows the results of the validation for various resources defined in the `roomvu-deployment.yaml` file. Each line starts with either "PASS" or "FAIL" followed by a message indicating the result of the validation check for a specific resource.

In this case, all the listed resources (`Deployment`, `Service`, `PersistentVolumeClaim`, `Secret`, and `HorizontalPodAutoscaler`) have passed the validation checks, as indicated by the "PASS" prefix and the resource name in parentheses.

For example:
* The `roomvu-deployment.yaml` contains a valid `Deployment` resource named `nginx-deployment`.
* It also contains a valid `Service` resource named `nginx-service`.
* Similarly, other resources like `mysql-deployment`, `mysql-pvc`, `mysql-service`, `laravel-deployment`, `laravel-pvc`, `laravel-service`, `mysql-secret`, and `nginx-hpa` are also present and successfully validated.

Overall, it seems that the `roomvu-deployment.yaml` file has been validated and found to contain valid Kubernetes resources according to the specified version and schema.


#To run this deployment.yaml file, follow the below steps:

* 1. Make sure you have Kubernetes cluster installed and running.
* 2. Open a command prompt or terminal window.
* 3. Navigate to the directory where the deployment.yaml file is located.
* 4. Run the following command to create the resources specified in the deployment.yaml file: 

  <pre>
   kubectl apply -f roomvu-deployment.yaml
  </pre>

# This will create all the necessary resources such as deployments, services, persistent volume claims, secrets, and horizontal pod autoscaler.

After running the above command, you can verify that the resources have been created by running the following commands:

* 1. To get the list of deployments, run:

<pre>
   kubectl get deployments
</pre>

* 2. To get the list of services, run:

<pre>
   kubectl get services
</pre>

* 3. To get the list of pods, run:
<pre>
   kubectl get pods
</pre>

# If everything is configured correctly, you should see output indicating that the resources have been created successfully.


# how to check the task is done!

To browse the web application named "roomvu-app" that is clustered on Kubernetes and **exposed** on port **80**, you first need to find the IP address of the ingress controller. Based on me, it sounds like the ingress controller is deployed on a Kubernetes cluster within my local network IP range of **192.168.122.0/24**.

Here's command to list all running Kubernetes pods:

<pre>
kubectl get pods -n <namespace>
</pre>

to check to list all the PersistentVolumeClaims (PVCs) for mysql and Laravel that exist in a cluster.

<pre>
kybectl get pvc
</pre>


Replace **<namespace>** with the namespace where the Roomvu app is deployed. If you're not sure what namespace to use, try **kubectl get namespaces** to list all available namespaces.

Once you have the name of the ingress controller pod, you can get its IP address using the following command:

<pre>
kubectl get pod <pod-name> -n <namespace> -o jsonpath="{.status.hostIP}"
</pre>

Replace **<pod-name>** and **<namespace>** with the actual pod name and namespace where the ingress controller is deployed.

Now that you have the IP address of the ingress controller, you can open a web browser and navigate to the IP address to access the Roomvu app. For example, my IP address of the ingress controller is **192.168.122.10**, you would enter **http://192.168.122.10** in your web browser.

Assuming the ingress controller has been properly configured with an Ingress resource for the Roomvu app, you should be able to access it at the root URL or the path specified in the Ingress rules. For example, if the Ingress rule specifies **/roomvu** as the path for the Roomvu app, you would enter **http://192.168.122.10/roomvu** in your web browser to access the app.

I hope this helps! Let me know if you have any further questions.

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=git,gitlab,github,githubactions,jenkins,nginx,kubernetes,docker,ansible,gcp,aws,azure,linux,mongodb,mysql,postgres,prometheus,laravel,py,django,flask,rabbitmq,redis," />
  </a>
</p>

call me on +98 939 63 10 462 or mail me  farshidrahimi.ca@gmail.com

* **Final and important explanation**

**This is changed and customized and named version 2, Version 2 of this product has the same features and functionality as Version 1, but with improved flexibility for customization thanks to the separation of manifest files.



**Considering that i saied, the submitted task will have more aspects of checking and understanding my technical values for you. I tried to explain the steps completely in order and in detail. Or in general terms (line by line). But in the end, my suggestion for doing this task professionally will be as follows:**

**Managing Docker images in Kubernetes can be done using a combination of Ansible, Terraform, and/or Puppet. Here are some steps you can follow to get started:**

*   Define your infrastructure as code (IaC): Decide which IaC tool you want to use - Ansible, Terraform, or Puppet - to define your infrastructure resources such as nodes, containers, and services.

*   Define the Docker image: Define your Docker image within your IaC code. You can specify the container image tag, repository location, and relevant configurations.

*   Build and push the Docker image: Use a Dockerfile to build your application, and then push it to a container registry such as DockerHub or Google Container Registry.

*  Deploy the image: Use your IaC tool to deploy your Docker image to Kubernetes. This can be done by defining a Kubernetes deployment YAML file that specifies the Docker image and any other required configuration settings.

* Automate the process: Once you have defined your IaC code to deploy your Docker images in Kubernetes, you can automate the entire process by using CI/CD tools such as Jenkins, GitLab CI, or CircleCI.

**By following these steps, you can manage your Docker image projects in Kubernetes using Ansible, Terraform, or Puppet.**



> **Warning**
> **Attention Task Tester:**
**Local** testing is often different from testing in a real **product environment**. I have tried to explain the details of all the stories involved in this test in every stage. However, due to the nature of many infrastructure cases, there may be a need to manually check and make some changes and versioning to this test, which is usually negotiable to finalize. Please bear with**me**. **Thank you**.
