# CI Pipeline: Java + Maven with Jenkins

This project demonstrates a Continuous Integration (CI) pipeline using Jenkins, Java 17, and Maven 3.9.

The pipeline automatically:

1.Checks out code from GitHub (via SSH).
2.Builds the Maven project.
3.Runs unit tests and publishes test results.
4.Packages the JAR and archives it as a Jenkins build artifact.

# Project Structure
CI-Java-Maven/
├─ Jenkinsfile          # CI pipeline definition
└─ app/                 # Java Maven project
   ├─ pom.xml           # Maven configuration
   ├─ src/
   │  ├─ main/java/...  # Application code
   │  └─ test/java/...  # Unit tests
   └─ target/           # Build outputs (JAR, reports)

# Jenkins Setup

## Tools Required:
JDK 17 → configured in Jenkins as jdk17
Maven 3.9 → configured in Jenkins as maven-3.9

## Pipeline Configuration:
Repository URL (SSH):
git@github.com:heycru/CI-Pipeline---Java---Maven.git
Credentials: github-ssh (SSH private key for GitHub)
Branch to build: main

## Stages:
1.Checkout → clones the repo.
2.Build & Test → runs mvn clean verify and collects JUnit reports.
3.Package & Archive → builds app-1.0.0.jar and archives it.

## Build Artifacts
After each successful build:
JAR file: app/target/app-1.0.0.jar
Test reports: app/target/surefire-reports/

## Run Locally
cd app
mvn clean verify   # build and run tests
mvn package        # produce the JAR
java -jar target/app-1.0.0.jar   # run the JAR (if main class defined)

## Status
CI pipeline: Working
Tests: Passing
Artifact: app-1.0.0.jar archived in Jenkins
