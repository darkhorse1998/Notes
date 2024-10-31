# Facts

1. Pipeline should be named as **.gitlab-ci.yml**
2. `stages` are required to be declared to set an order of execution of the jobs. With that jobs might trigger simultaneously and cause errors.
3. Jobs would run on different agents, and hence won't exchange any data between them. If there are any inter dependent resources involved, the jobs would fail.
4. To ensure that the jobs save the required data for other jobs, we define **artifacts**
5. Gitlab would also check the pipeline if the format is proper of not.