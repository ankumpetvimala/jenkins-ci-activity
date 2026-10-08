# Jenkins CI Pipeline – Hands-on Activity

**Organization:** Davine Technologies  
**Role:** Junior DevOps Engineer  
**Project:** Continuous Integration (CI) Pipeline using GitHub and Jenkins

## 1. Objective

Create a Jenkins CI pipeline that checks out application code from GitHub, builds the project, runs tests, and validates required files. Configure a GitHub webhook so that a new push automatically triggers the pipeline. Demonstrate a controlled failure, troubleshoot it using Jenkins Console Output, fix the issue, and run the pipeline successfully.

## 2. Repository

- **GitHub Repository:** https://github.com/ankumpetvimala/jenkins-ci-activity
- **Branch:** `main`
- **Build Tool:** Apache Maven
- **CI Server:** Jenkins
- **Runtime:** Java
- **Pipeline Definition:** `Jenkinsfile`

## 3. Pipeline Workflow

```text
Developer
   |
   | git push
   v
GitHub Repository
   |
   | GitHub Webhook
   v
Jenkins
   |
   v
Checkout
   |
   v
Build (mvn clean package -DskipTests)
   |
   v
Test (mvn test)
   |
   v
Validation (check required project files)
   |
   v
Pipeline Result: SUCCESS / FAILURE
```

## 4. Pipeline Stages

1. **Checkout** – Retrieves the source code from the GitHub repository.
2. **Build** – Builds the Maven project and packages the application.
3. **Test** – Runs the project's automated tests.
4. **Validation** – Checks that required files and build output exist, such as `pom.xml`, the `target` directory, and the application source file.

> This pipeline runs on Windows, so Windows-compatible Jenkins `bat` commands are used instead of Linux-only `sh` or `test` commands.

## 5. GitHub Webhook Configuration

1. Open the GitHub repository.
2. Go to **Settings → Webhooks → Add webhook**.
3. Enter the Jenkins webhook endpoint configured for the Jenkins installation, commonly:
   `http(s)://<jenkins-host>/github-webhook/`
4. Select **application/json** as the content type.
5. Select the push event.
6. Save the webhook and verify its delivery status in GitHub.

**Note:** Replace `<jenkins-host>` with the reachable address of your Jenkins server. A Jenkins instance running only on `localhost` is not reachable by GitHub over the public internet; use a properly secured, reachable endpoint or a suitable webhook tunnel for a local practical.

## 6. Troubleshooting: Failure → Resolution

### Issue 1: `sh` command not found

**Error:** `Cannot run program "sh"`  
**Cause:** Jenkins was running on Windows, but the pipeline used a Linux shell command.  
**Resolution:** Replaced Linux `sh` steps with Windows `bat` steps.

### Issue 2: Maven command not recognized

**Error:** `'mvn' is not recognized as an internal or external command`  
**Cause:** Maven was not available in the environment used by the Jenkins service.  
**Resolution:** Downloaded and extracted Apache Maven, added Maven's `bin` directory to the Windows system `PATH`, verified `mvn -version`, and restarted Jenkins so it could read the updated environment.

### Issue 3: Validation command failed

**Error:** `'test' is not recognized as an internal or external command`  
**Cause:** The Validation stage used the Unix command `test -f`, which is not a standard Windows Command Prompt command.  
**Resolution:** Replaced it with Windows-compatible checks using `if exist`, for example:

```bat
if exist pom.xml (echo pom.xml found) else (echo pom.xml missing & exit /b 1)
if exist target (echo target folder found) else (echo target folder missing & exit /b 1)
if exist src\main\java\App.java (echo App.java found) else (echo App.java missing & exit /b 1)
```

### Issue 4: Git push rejected

**Error:** `rejected ... (fetch first)`  
**Cause:** The remote branch contained changes that were not in the local branch.  
**Resolution:** Integrated remote changes before pushing:

```bat
git pull --rebase origin main
git push origin main
```

If a conflict occurs, resolve it before continuing; do not force-push as a routine fix.

## 7. Test Result

During troubleshooting, Maven reported:

- Tests run: **1**
- Failures: **0**
- Errors: **0**
- Skipped: **0**
- Maven result: **BUILD SUCCESS**

The pipeline initially failed in later stages because of Windows/Linux command incompatibilities. The commands were updated for Windows and the pipeline was run again.

**Final status:** Record `SUCCESS` only after the latest Jenkins build completes all stages successfully. Attach the actual successful build screenshot as evidence.

## 8. How to Demonstrate Automatic Triggering

1. Make a small change to a tracked project file.
2. Commit and push the change to the `main` branch.
3. Confirm that GitHub's webhook delivery succeeds.
4. Open Jenkins and verify that a new build starts automatically.
5. Review the stage results and Console Output.

## 9. Submission 

Save screenshots and add them to an `Screenshots/` folder in the repository, or submit them separately as required.

- [ ] GitHub repository link
- [ ] GitHub Webhook configuration and successful delivery
- [ ] Successful Jenkins pipeline with all stages
- [ ] Failed Jenkins pipeline from the controlled error
- [ ] Console Output showing the error
- [ ] Additional Validation stage
- [ ] Final successful run after fixing the error

Suggested filenames:

```text
Screenshots/
├── github-webhook.png
├── pipeline-success.png
├── pipeline-failure.png
├── console-output-error.png
└── validation-stage.png
```

## 10. Conclusion

This activity demonstrates a basic CI workflow using GitHub, GitHub Webhooks, Jenkins, Java, and Maven. The pipeline separates checkout, build, test, and validation tasks. Troubleshooting highlighted the importance of matching pipeline commands to the operating system, configuring Maven in the Jenkins service environment, and reviewing Console Output to locate failures.

---

**Repository:** https://github.com/ankumpetvimala/jenkins-ci-activity  
**Branch:** `main`
