# Configuring Anchore and Jenkins (with Ansible and AWS)
Step-by-step instructions: Coming soon!

## Optional - Enable feature branch merge requests to trigger your Jenkins job
As I was performing this step on my machine, I noticed most of the documentation around this was "out of date" so I wanted to make this as easy as possible. _Note:_ you may have to open up the ingress rules on your EC2 instance in order for the GitLab API to communicate with Jenkins.
1. In Jenkins, inside your job's **Configure** settings, underneath **Build Triggers**
  * _check_ the box that says **Build when a change is pushed to GitLab**
  * _copy_ the **GitLab webhook URL** somewhere safe! (you'll need this later!)
	  - _example:_ `http://x.xx.xxx.xxx:8080/project/<YOUR_JOB_NAME>`
  * Underneath that, _check_ the box that says **Opened Merge Request Events**
	* _click_ **Save**
2. In GitLab, create an access token:
  * _go_ to **User Settings** > **Access Tokens**
	* _create_ a token with “read_repository” scope
	* _copy_ your personal access token somewhere safe! (you’ll need this later!)
3. Still in Gitlab, create a webhook:
  * _go_ to **Project settings** > **Webhooks**
	* _paste_ your **GitLab webhook URL** from the previous step
	* _check_ the **Merge request events** box
	* _uncheck_ the **Enable SSL verification** box
	* _click_ the **Add webhook** button
  * _scroll down_ and _test_ that your merge request events webhook connection returns `Hook executed successfully: HTTP 200`
3. Back in Jenkins, inside **Manage Jenkins** > **Configure System**
  * _scroll down_ to the ‘GitLab’ settings
  * _enter_ a **Connection name** (this can be anything you want)
  * _update_ the **GitLab host URL** to `https://gitlab.com`
  * _add_ a **Credential**
	  - _change_ **Kind** to “GitLab API token"
	  - _paste_ your **API Token** from the previous step
	  - _click_ **Add**
  * _click_ **Test Connection** and verify that it returns “Success"
  * **Save** your system settings

For more help with connecting Jenkins and Gitlab, here are a few resources:
- [GitLab Jenkins CI service](https://docs.gitlab.com/ee/integration/jenkins.html)
- [Create A Continuous Integration Pipeline With GitLab And Jenkins](https://docs.bitnami.com/aws/how-to/create-ci-pipeline/)

_Additional Resources/Demos:_
- [Securing your Jenkins CI/CD Container Pipeline with Anchore (in under 10 minutes)](https://jenkins.io/blog/2018/06/20/anchore-image-scanning/)
- [Integrating Anchore Scanning into Jenkins Pipeline via Jenkinsfile](https://anchore.com/integrating-anchore-scanning-into-jenkins-pipeline-via-jenkinsfile/)
- [Using Docker with Pipeline](https://jenkins.io/doc/book/pipeline/docker/)