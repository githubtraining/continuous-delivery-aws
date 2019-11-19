## GitHub Enterprise Server (GHES) installation notes

🚫 | This course needs features not yet supported by GHES.
--- | ---

This course makes use of [GitHub Actions](https://github.com/features/actions), a feature that is currently available for GitHub.com but not available on GHES.

You may still make significant modifications to the course to work with a third-party code analysis tools.

### Course dependencies

The following are dependencies of the course. The course may continue to work without these dependencies, but learners won't experience the course as designed.

| Dependency                                                                                                                      | Required? | Reason                                                                                                                                           | Alternative                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------------------------------------|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GHES must be able to reach githubusercontent.com                                                                                | Yes       | Images used throughout the course are served from this domain. Learners will find broken images required to take the course without this access. | Manually download the images referenced in the `responses/` folder, upload them to an accessible domain, and replace the images in the `responses/` folder.                                    |
| Learner must be able to reach github.com and outside web                                                                        | No        | Links are provided to resources that live on the outside web.                                                                                    | Without access to resources on the outside web, learners will reach blocked resources. You can change the links to these resources in the `responses/` folder, and in the template repository. |
| [GitHub Pages](https://help.github.com/en/enterprise/admin/installation/configuring-github-pages-on-your-appliance)             | No        | Used to let the learner work on a realistic, published web page.                                                                                 | Remove the first step in `config.yml`, and adjust responses in the `responses/` folder to remove any mentions of GitHub Pages. |