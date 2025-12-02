# sumextprct



























* Explain the Functional and Non-Functional requirements of Online Course Reservation system.
* Construct a Class diagram for Online Course Reservation System.
* Illustrate the scaling of a container named mysql through Kubernetes.
* create a complete Jenkins Scripted Pipeline that will automatically clone the source code from GitHub, build it, test it, and archive the results.

FUNCTIONAL REQUIREMENTS (FR):
1. User Registration & Login
   - Users can create accounts and log in to the system.

2. Course Browsing & Search
   - Students can view and search available courses.

3. Course Details
   - Students can open a course to see description, schedule, and availability.

4. Course Reservation
   - Students can reserve a seat in a course offering.

5. Cancel Reservation
   - Students can cancel an existing reservation.

6. Instructor Course Management
   - Instructors can create, update, and manage their courses.

7. Admin Management
   - Admin can manage users and courses.

8. Notification (optional)
   - System notifies users about confirmations or cancellations.

NON-FUNCTIONAL REQUIREMENTS (NFR):
1. Performance
   - System should load pages within 2–3 seconds.

2. Scalability
   - System must support many users and growing course data.

3. Security
   - Secure login, encrypted passwords, and protection against attacks.

4. Reliability
   - System should be available with minimal downtime.

5. Usability
   - Interface must be easy, intuitive, and user-friendly.

6. Maintainability
   - System should be easy to update, fix, and extend.

7. Compatibility
   - Must work on major browsers and devices.

8. Data Integrity
   - Course availability and reservation data must always be accurate.
Summary
Functional Requirements
➡ Describe what the system does Examples: Register user, search, book, cancel, pay, notify, manage reservations.
Non-Functional Requirements
➡ Describe how the system behaves Examples: Performance, security, reliability, availability, usability.CLASS DIAGRAM – SHORT DEFINITION & REQUIREMENTS

DEFINITION:
A Class Diagram is a UML diagram that shows the classes of a system, their attributes, methods, and the relationships between them. It represents the structural blueprint of an object-oriented system.

REQUIREMENTS:
1. Classes
   - Represent key entities
   - Contain attributes (data)
   - Contain methods (functions)

2. Relationships
   - Association (link between classes)
   - Inheritance (parent → child)
   - Aggregation/Composition (whole–part)
   - Multiplicity (1..*, 0..1, etc.)

3. Visibility (optional)
   - + public
   - - private
   - # protected

4. Naming Rules
   - Class names = nouns
   - Method names = verbs

5. Purpose
   - Shows structure of system
   - Helps in object-oriented design



￼
//Kubernets
kubectl create deployment mynginx --image=nginx
kubectl get deployments
kubectl expose deployment mynginx --type=NodePort --port=80 --target-port=80
kubectl scale deployment mynginx --replicas=4
kubectl get deployments
kubectl get pods
kubectl port-forward svc/mynginx 8081:80
// After this open another powershelgl
minikube dashboard in powershell
//Return to original pwshll
kubectl delete deployment mynginx
kubectl delete service mynginx
minikube stop
// Second way
# LAB: Deploying and Scaling a MySQL Container Using Minikube
STEP 1 — Start Minikube
minikube start
# Verify Kubernetes is running
kubectl get nodes
# STEP 2 — Create a folder for Kubernetes YAML files
cd $HOME\Desktop
mkdir k8s-lab
cd k8s-lab
# STEP 3 — Create the MySQL Deployment YAML file
# Run the command below to open Notepad:
notepad mysql-deployment.yaml
# Paste the YAML below INSIDE Notepad and save:
# ------------------- mysql-deployment.yaml ----------------
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "password123"
        ports:
        - containerPort: 3306
# STEP 4 — Deploy MySQL to Kubernetes
kubectl apply -f mysql-deployment.yaml

# Check deployment and pods
kubectl get deployments
kubectl get pods
# STEP 5 — SCALE the MySQL application
# Scale UP to 3 replicas
kubectl scale deployment mysql --replicas=3
# Verify scaling
kubectl get pods
# STEP 6 — SCALE DOWN (optional)
kubectl scale deployment mysql --replicas=1
kubectl get pods
# STEP 7 — CLEANUP (optional)
kubectl delete -f mysql-deployment.yaml
minikube stop

 /// scripted pipeline

pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME'
    }
    stages {
        stage('git repo & clean') {
            steps {
                //bat "rmdir /s /q SampleMavenJavaProject"
                bat "git clone https://github.com/budarajumadhurika/SampleMavenJavaProject"
                bat "mvn clean -f SampleMavenJavaProject"
            }
        }

        stage('install') {
            steps {
                bat "mvn install -f SampleMavenJavaProject"
            }
        }

        stage('test') {
            steps {
                bat "mvn test -f SampleMavenJavaProject"
            }
        }

        stage('package') {
            steps {
                bat "mvn package -f SampleMavenJavaProject"
            }
        }
    }
}
