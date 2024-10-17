<div align="center">
  <br />
    <a href="https://youtu.be/R8CIO1DZ2b8" target="_blank">
      <img src="https://github.com/adrianhajdin/zoom-clone/assets/67959015/f09a8421-67d3-45ce-b9bc-a791cdc2774b" alt="Project Banner">
    </a>
  
  <br />

  <div>
    <img src="https://img.shields.io/badge/-TypeScript-black?style=for-the-badge&logoColor=white&logo=typescript&color=3178C6" alt="typescript" />
    <img src="https://img.shields.io/badge/-Next_JS-black?style=for-the-badge&logoColor=white&logo=nextdotjs&color=000000" alt="nextdotjs" />
    <img src="https://img.shields.io/badge/-Tailwind_CSS-black?style=for-the-badge&logoColor=white&logo=tailwindcss&color=06B6D4" alt="tailwindcss" />
  </div>

  <h3 align="center">A Zoom Clone</h3>
</div>

## 📋 <a name="table">Table of Contents</a>

1. 🤖 [Introduction](#introduction)
2. ⚙️ [Tech Stack](#tech-stack)
3. 🤸 [Quick Start](#quick-start)
4. 🔧 [CI/CD Pipeline](#jenkins)

## <a name="introduction">🤖 Introduction</a>

This repository is part of a collection of three repositories that follow GitOps principles to deploy a Zoom clone application to an AWS EKS cluster. The application was originally created by [Adrian Hajdin](https://github.com/adrianhajdin/zoom-clone).

The other repositories in this collection are:
- [Infrastructure as Code (IaC) repository](https://github.com/shadyosama9/Zoom-Clone-Infra)
- [Kubernetes configuration repository](https://github.com/shadyosama9/Zoom-Clone-K8s)

This repository includes the Dockerfile and Jenkinsfile, which are used to containerize the application and set up the CI/CD pipeline.


## <a name="tech-stack">⚙️ Tech Stack</a>

- Next.js
- TypeScript
- Clerk
- getstream
- shadcn
- Tailwind CSS
- Docker
- Jenkins

## <a name="quick-start">🤸 Quick Start</a>

Follow these steps to set up the project locally on your machine.

**Prerequisites**

Make sure you have the following installed on your machine:

- [Git](https://git-scm.com/)
- [Node.js](https://nodejs.org/en)
- [npm](https://www.npmjs.com/) (Node Package Manager)

**Cloning the Repository**

```bash
git clone https://github.com/adrianhajdin/zoom-clone.git
cd zoom-clone
```

**Installation**

Install the project dependencies using npm:

```bash
npm install
```

**Set Up Environment Variables**

Create a new file named `.env` in the root of your project and add the following content:

```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=

NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up

NEXT_PUBLIC_STREAM_API_KEY=
STREAM_SECRET_KEY=
```

Replace the placeholder values with your actual Clerk & getstream credentials. You can obtain these credentials by signing up on the [Clerk website](https://clerk.com/) and [getstream website](https://getstream.io/)

**Running the Project**

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to view the project.

---

### Running the Project with Docker Locally

**Prerequisites**

Make sure you have Docker installed on your machine. You can download and install Docker from the official 

- [Docker website](https://www.docker.com/get-started).

**Set Up Environment Variables**

Create a new file named `.env` in the root of your project and add the same content as mentioned above.

**Building the Docker Image**

```bash
docker build -t <image-name> .
```
Replace `<image-name>` with a name of your choice for the Docker image.

**Running the Docker Container**

```bash
docker run -p 3000:3000 <image-name>
```
This command will start the app, making it accessible at [http://localhost:3000](http://localhost:3000) in your browser.



---

## <a name="jenkins">🔧 CI/CD Pipeline</a>

Jenkins is used with a declarative approach to establish the CI/CD pipeline for the application.

**Prerequisites**

- An AWS account 
- Make sure you created the infrastructer in [Infrastructure as Code (IaC) repository](https://github.com/shadyosama9/Zoom-Clone-Infra)

**Jenkins Installation**

- Login to the Jenkins EC2 machine.
- Run the following command to update the package list:

```bash
sudo apt update
```

- Install JDK:

```bash
sudo apt install openjdk-21-jdk
```

- Install Jenkins:
Follow the instructions in the [Jenkins installation guide](https://www.jenkins.io/doc/book/installing/linux/#debianubuntu).

- Install Docker:
Follow the instructions in the [Docker installation guide] (https://docs.docker.com/engine/install/ubuntu/).

- Add Jenkins to the Docker group:

```bash
sudo usermod -aG docker jenkins
```
- Install Trivy:
Follow the instructions in the [Trivy installation guide] (https://aquasecurity.github.io/trivy/v0.18.3/installation/).




