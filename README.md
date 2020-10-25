Cloud-DevOps-Nanodegree-Capstone-Project
This is the capstone project of Cloud DevOps Nandegree Program. It demonstrates the ability to use AWS, Jenkins, Docker, Kubernetes, build a CI/CD Pipeline, deploy EKS Cluster as IaC.

Instructions for Setup
-------------

Create an ubuntu VM and install jenkins on it.

Configure awscli

Install Docker, Lint, EKS packages

Create Kubernetes cluster using aws EKS on your Ubuntu VM

Create github repository for the project

Configure jenkins with credentials for github, aws and docker

Write a docker file to build the nginix image with customised intex,html

Write jenkins pipeline for lint,build and push docker image to docker hub

Add jenkins pipeline stage for deploying customaised docker conatainer to the kubernetes cluster created.

List the pods, services and  created in the kuberntes cluster 

Prune the docker image.
