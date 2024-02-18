# OpenAPI Kotlin client publisher
A template for a repository that tracks upstream repository for releases, and builds and publishes a multiplatform Kotlin client
based on released OpenAPI specification with the same release name.

## Usage
Create a new repository using this template. 

Make the following changes in the repository settings:\
Under "Secrets and variables" -> Actions
* Add variable UPSTREAM_REPOSITORY in the format of OWNER/REPOSITORY.
* Add secret PERSONAL_TOKEN secret that contains Github access token that has permissions to clone the repository and publish packages. Using the new fine-grained personal access tokens this is contents: Read and write.\
 
Navigate to Actions -> General:
* Change "Workflow permissions" to "Read and write permissions".

In the repository, fill in empty values in generator-config.yaml.

The workflow that checks for new releases is run on push and then daily and the publishing workflow is run when necessary.

The artifact is published to GitHub packages in the repository. Check out how to use it at https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-gradle-registry#using-a-published-package

## Limitations
Github doesn't trigger workflows if the trigger came from another workflow that uses autegenerated GITHUB_TOKEN. 
Therefore the monitoring workflow needs for a separate token that creates the release.

Specification is assumed to be in the first asset of the release. Modify `jq` filter in step "Generate client code" if that's not correct.

If there are multiple releases a day in the upstream repository, only the latest is detected and built. Chron job timing can be adjusted if necessary, or releases created manually if needed.
