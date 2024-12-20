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
13. To disable a job, add a dot (.) before the job name. Example:
```yaml
.job1:
    script:
        ...
```
14. Anchors can be used in YAML to inherit properties and re-use them. It can be named anything. Example:
```yaml
name: &anchor David
upn: *anchor

base: &base
    city: a
    country: b

person1:
    <<: *base # will insert city and country from base object
    name: Jason
```
15. Gitlab has many pre-defined environment variables related to the project, pipelines etc. which can be found [here](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)
16. Pre-defined Gitlab variables can be used by `$<variable_name>`. Example: `echo $CI_COMMIT_AUTHOR`
17. Pipelines can be scheduled through Pipeline Schedules cron settings.
18. Gitlab can use caching to make the build process faster. We can specify cache at job level or at global level. Caches can be cleared using **Clear Runner Caches** button. Example:
```yaml
cache:
    key: ${CI_COMMIT_REF_SLUG} #Gets the current branch
    paths:
        - node_modules/
```
19. Artifacts are used to pass data between stages/job. Caches are used as temporary storage for project dependencies.
20. Environments in Gitlab allows us to control the Continuous Delivery/Continuous Deployment.
21. Continuous delivery vs Continuous Deployment - \
Continuous Delivery: It involves building, testing, and staging code changes, but a developer must manually decide when to deploy to production. This process can be beneficial when a company needs to frequently release changes to production, but still wants some human intervention.
Continuous Deployment: Automatically releases code changes to production after they pass all stages of the production pipeline. This process can help accelerate the feedback loop with customers and reduce pressure on the development team.
22. Environments can be used in pipelines through the environment parameter. In the Gitlab UI we can track about the deployments in each of the environments.
23. Variables can be declared in global or job level.
24. Manual intevention can be injected in the pipelines by `when: manual` in the respective jobs. This can be useful when there is a need of manual intervention or approval before deployment. For example, production drops.
25. In case, when there are jobs after manual intervention jobs, they might fail as they won't wait. We can resolve this by adding `allow_failure: false` to the job which has manual intervention.
26. If you want the job to run only when it runs from a certain branch/branches, it can be done using `only`. Example:
```yaml
job1:
    only:
        - master
    ...
```
27. To make a job run when a merge request is raised, we can use `only`. Example:
```yaml
job1:
    only:
        - merge_requests
    ...
```
28. Branches can be protected by setting permissions for 'Allowed to push' to 'No one' for the main/master branch in Settings -> Repository -> Protected Branches
29. You can create dynamic environments for each merge request to allow developers and QA to test the deployment on each merge request.
30. Setting the variable `GIT_STRATEGY` to `none` will make sure that the job or the pipeline will not try to clone the repository (from whichever branch it is running on). Exmaple:
```yaml
job1:
    variables:
        GIT_STRATEGY: none
```
31. `before_script` can be declared at job/global level, and runs before the main script. `after_script` works in a similar manner and is executed after the main script, however it is different than 'before_script' as its commands are executed in a separate context and local working directory is set back to default.
32. Environments can be stopped and cleaned up manually/dynamically. Example:
```yaml
deploy:
    environment:
        name: dev
        url: abc.com
        on_stop: stop environment
    ...

stop environment:
    only:
        - merge_requests
    variables:
        GIT_STRATEGY: none
    when: manual
    environment:
        name: dev
        action: stop
    ...

```
33. Organizing variables in groups can help share those variables in other projects and set permissions accordingly.
34. In case of AWS authentication, simply creating the variables in the Gitlab variables is enough as thy will be injected as environent variables. Variables defined in the UI are automatically exposed as environment variables within the job environment.
35. Setting `when: always` under `artifacts` will ensure that the artifacts are saved and can be viewed even if the job fails. This is important in case of testing, where tests might fail but we would need the reports to understand what is wrong.
36. Specifying `reports` in artifacts can be useful to publish tests as reports in the artifact page. Example, for junit tests \
```yaml
job1:
    ...
    artifacts:
        when: always
        paths:
            ...
        reports:
            junit: ...
```
37. Gitlab Pages can be used to host static pages like reports. This can be used to publish test results.
