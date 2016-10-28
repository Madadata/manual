# Release Model

## Cutting a new release

These are the steps to be followed when cutting a release. However, you should be scrutinizing the process yourself when designing the release model for a specific project, because they can differ a lot, e.g. depending on whether the project is built and distributed using Docker, CI and/or CDN only.

### finishing up unclosed issues

First step is to always check for unclosed issues and pull requests that you want to include in your new release. This means you should take a look at the issues and pull requests page in GitHub and also talking to project stakeholders - everyone should be clear that you are cutting a new release and what to be included and excluded (for future releases).

### deciding on the impact

Following sematic versioning, you should decide whether you want to cut a build release, minor release or major release. For minor release you should notify all consumers of your APIs for possible impact. For major release, ideally you should notify everyone.

### sync and branch from master a new release branch

Make sure you are locally up-to-date with `master` and then `git checkout` a new release branch, ideally named `release/a.b.c`.

Then you can go ahead and edit all manifist files, e.g. `version` in `package.json` and `<version>` in `pom.xml`. Make sure you edit **all** of them otherwise you will end up with nasty version inconsistency. Write a script or use `find ... -exec sed` to help you do this.

Make sure you only change version and then commit. Push the new branch with that commit to remote, **let the CI pass**, and then `git tag a.b.c`, and then push the tag using `git push --tags`. If you use GPG key, use `git tag -s` to sign your commit as well.

### post tagging

After you bumped the version, continue on the release branch. You can edit again the `<version>` with the trailing `-SNAPSHOT` if you are using Java and maven (not for node though).

Generate a new CHANGELOG using the generator tool. Then go ahead and commit everything. This closes the release branch - so after the PR from release branch to master is merged, you are done, hooray!

### Npm Publish Checking List

- [ ] checkout a new release branch `release/a.b.c`
- [ ] edit `version` in `package.json`
- [ ] create an `.npmignore` file if you didn't, otherwise npm will take your `.gitignore` as `.npmignore`.
- [ ] **Private Package**: make sure your package name in `package.json` is prefixed with your scope, like `@madadata`.
- [ ] commit all your changes
- [ ] push to github and **let the CI pass**
- [ ] tag your commit by `git tag a..b.c` and push the tag by `git push --tags`
- [ ] npm publish
