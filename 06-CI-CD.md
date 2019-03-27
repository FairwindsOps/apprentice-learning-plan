# Part VI: CI/CD

## Objectives
By the end of this module, the SRE will:
- Explain Continuous Integration and Continuous Deployment
- Setup a CI/CD pipeline 

## Deliverables 
Complete the project described below. Answer the questions, then schedule a time to meet with your mentor and discuss the what you've built. 

## Exercise 
As you complete the project below, consider these questions: 
- What is Continuous Integration
  - Integration of what?
- What is the D in CD?
- What are the benefits and risks of CI/CD over traditional, manual software deployment?

--- 
- Create a new VM in AWS in a public subnet with an attached ElasticIP (see [Cloud Computing](./05-Cloud-Computing.md))
- Create a local git repository
- Create a simple webservice and deploy it to the VM.
  - What did you choose to deploy?
  - Consider how you might deploy a service with/without Docker
  - How does Docker change the deployment and development?
  - How would you ensure the service continues to run/restart when it fails?
- Update webservice and deploy a new version
  - What are the key steps involved in deploying the new version to your VM?
  - What logins and secrets are required by the VM to access code/artifacts/containers?
  - If you had a data service, what other considerations might there be?
  - How will you confirm that the new version is indeed running?
- With the commands you used to deploy the second version of the service, create a script.
- Use the script to deploy the service again.
  - Excercise this process a few times manually
  - Consider what it might involve to trigger the deploy script automatically after a code commit
- CircleCI setup:
  - Create a remote repository and push your code to it.
  - Visit https://circleci.com/
  - https://circleci.com/docs/2.0/sample-config/#sample-configuration-with-sequential-workflow
  - https://circleci.com/docs/2.0/basics/
  - https://circleci.com/docs/2.0/sample-config/
