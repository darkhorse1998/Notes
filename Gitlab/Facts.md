# Facts

1. Pipeline should be named as **.gitlab-ci.yml** and it should be places at the root of the project.
2. `stages` are required to be declared to set an order of execution of the jobs. With that jobs might trigger simultaneously and cause errors.
3. Jobs would run on different agents, and hence won't exchange any data between them. If there are any inter dependent resources involved, the jobs would fail.
4. To ensure that the jobs save the required data for other jobs, we define **artifacts**
5. Gitlab would also check the pipeline if the format is proper of not.
6. Gitlab architechture includes a Gitlab server and Gitlab runners. The server is basically used to manage all the things realated to he project and the configurations are saved in the database. The Gitlab server will deligate te work of running the pipelines to the Gitlab runners and store any artifacts if required. Runners can be scaled up or down. Runners can also be configured to use shared runners or cutomized runners according to the use-case.
![image](https://github.com/user-attachments/assets/68542bc3-db2f-46d3-8850-e72e90470688)

7. If not specified, then a job will automatically be assigned to **test** stage.
8. Gitlab can be used as a SaaS hosted by Gitlab or it can also be used through self-managed hosted instance on-premise or on public cloud.
9. To use a specific image for the runner, use the `image` attribute. Example:
```yaml
<job-name>:
    image: <image-name>
    script:
        ...
```
10. To execute jobs in parallel, use the same stage name.
11. To make steps in the script run in background and not block the terminal or the runner, we use ampersand sign (&). Example: `- gatsby serve &`
12. To make all the jobs use the same image, declare `image` globally and use local `image` tags in the jobs in case you need specific images. Example:
```yaml
image: node:lts

job1:
    script:
        ...
job2:
    image: alpine
    script:
        ...
```
13. 