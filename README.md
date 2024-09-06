# springboot-demo Implementation
## Result
![Screenshot 2024-09-06 194503](https://github.com/user-attachments/assets/bcbeb233-d296-415f-97d6-9802fc23abe4)


## 1. Run in the Local
### Requirement
Installing Java 17:

    sudo apt update
    sudo apt install openjdk-17-jdk

Installing Maven:
    
    sudo apt update
    sudo apt install maven



Clean and Install the Project:

    mvn clean install


Compile the Project:

    mvn compile

Build the Project:

    mvn package

Run the Application:
Or, navigate to the target directory and run the packaged JAR file:
    
    cd target
    java -jar Mayurdevops-0.0.1-SNAPSHOT.jar


Build Issues:
If the build fails, run Maven with detailed logging to diagnose the problem:

    mvn clean install -X

## 2. Run in Docker

Building and Running the Docker Image

        docker build -t Mayurdevops:latest .

Run the Docker Container:

        docker run -p 8080:8080 Mayurdevops:latest

        
