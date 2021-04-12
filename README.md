### Readme
In this project we are going to do the following:

- Setup an Ubuntu VM
- Install openjdk11, latest Jenkins, latest PostgreSQL and latest Docker
- Create PostgreSQL user
- Run SonarQube in a Docker container
- Jenkins post-installation configuration and plugins installation
- Build pipeline with Blue Ocean
- Run pipeline and trigger build on a per-minute scan base

### Step-by-Step instruction:
1. Put the Vagrantfile provided under your project folder.
2. In the CLI: ``` vagrant up```. This step will provision a VM with 4GB of memory, 2 cpu cores, openjdk11, latest Jenkins, latest PostgreSQL and latest Docker installed
3. Create PostgreSQL user and database for SonarQube:

```> sudo passwd postgres```

Enter the password for user “postgres”
```
> su - postgres
> psql
> CREATE USER sonar WITH ENCRYPTED PASSWORD 'password';
> CREATE DATABASE sonarqube;
> GRANT ALL PRIVILEGES ON DATABASE sonarqube to sonar;
> \q
> exit
```
5.Run SonarQube Docker container:
```
> sudo docker pull sonarqube:8.8-community
> sudo docker volume create --name sonarqube_data
> sudo docker volume create --name sonarqube_logs
> sudo docker volume create --name sonarqube_extensions
> sudo sysctl -w vm.max_map_count=262144
> sudo docker run -d --name sonarqube \
    --network "host"\
    -e SONAR_JDBC_URL=jdbc:postgresql://localhost/sonarqube \
    -e SONAR_JDBC_USERNAME=sonar \
    -e SONAR_JDBC_PASSWORD=password \
    -v sonarqube_data:/opt/sonarqube/data \
    -v sonarqube_extensions:/opt/sonarqube/extensions \
    -v sonarqube_logs:/opt/sonarqube/logs \
    sonarqube:8.8-community
```
By now you can access Jenkins in your browser at localhost:8080 and SonarQube at localhost:9000.

The Default SonarQube username and password are both "admin". To access Jenkins you can follow the instruction the first time you go to the Jenkins URL.

6.You still need to generate the SonarQube user token, , install SonarQube and Blue Ocean plugin, maven tool, create a pipeline in Blue Ocean by interacting with the web interfaces. Refer to the screen shots for more detail.

### Answers to some Questions
1. Document your CI/CD process. What are the sequence of steps in the process? What are the inputs and results of each step? You should document this process as though you were describing it to a team of software developers.

- Steps and results are described in the previous section


2. What technologies (e.g. Jenkins plugins) did you select to support your CI/CD process and why did you select them? For each plugin you should briefly describe what it does, what results it generates, and how it can be used to assess some aspect of quality.
- SonarQube Scanner: I originally want to use some lightweight tools such as Checkstyle and Findbugs to do the static analysis, but Jenkins disables plugins for these tools for security reasons, so I have no choice but use SonarQube. Setting up SonarQube is a bit cumbersome and you cannot view the analysis report directly in Jenkins. However, the tool itself is good to use because it is free, supports multiple language and provides thorough bug reports.

3.What metrics do you think are most useful for your process and why?
- Number and criticality of bugs: this could directly prevent the program from functioning correctly(such as divided by zero).
- Number and criticality of code smells: this could sometimes cause the program produce unexpected result(such as never-used variables, indicating forgotten parameters.)
- Test cases passed: this tells me the program execute at least as the test intended.
4. Reflect on your process; specifically, in your opinion which tools are the most useful and why?
- Blue Ocean: helps me build the pipeline step-by-step with ease.
- SonarQube: gives thorough bugs and code smells report and explains why each of them is important and suggest how to fix
