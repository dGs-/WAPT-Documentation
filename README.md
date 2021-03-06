WAPT Documentation
==================

This is the documentation repository of the WAPT project. The documentation is provided under the licence CC-BY-SA.

* Official Site : https://wapt.fr/
* Documentation : https://wapt.fr/en/doc/
* Official git repo : https://github.com/tranquilit/WAPT

How to contribute?
==================

You may want to have a look at our contribution guide: https://www.wapt.fr/en/doc/Contribute/index.html


How to push documentation to public ?
=====================================

Pushing the documentation through a rsync is bad.

To publish WAPT documentation to public, a 'release-' tag must be set using git.

Once a release tag has been set, it creates a tagged build which must be launched manually in Jenkins.

```bash
git add *
git commit -m 'I have checked it builds without errors, trust me'
git push
git tag -a release-1.8.1.6694-doc -m "WAPT documentation publish for 1.8.1.6694"
git push origin --tags
```

Once that tag has been pushed, in Jenkins you have to re-scan the multibranches project and go to `Tags` tab, select your tag and build it.

Wait for the entire build to go through, it will publish everything according to JenkinsFile procedure.