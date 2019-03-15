# Job lifecycle
List of all possible job states:
* __idle__: the job is not yet running and has not yet been considered for deployment
* __deployment__: the infrastructure is being deployed
* __ready__: the job is not yet runnng but the infrastructure has been deployed
* __running__: the job is runing
* __deleted__: the job has been deleted by the user
* __killed__: the job has been killed, for example it had been running for too long
* __completed__: the job has completed successfully
* __failed__: the job failed, for example the container image could not be pulled

