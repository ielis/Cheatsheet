# Google Cloud Platform


## Cloud Shell

### Add account

Add new account by running:

```bash
gcloud auth login
```

### Setup configuration

Set up configuration, one configuration per project.

---

List configurations:

```bash
gcloud config configurations list
```

Activate a configuration:
```bash
gcloud config configurations activate blabla
```
> Assuming that `blabla` configuration exists

Create a configuration `blabla`, associate the configuration with the account `abc@example.com`, and set project, compute zone and compute region.
```bash
gcloud config configurations create blabla
gcloud config set account abc@example.com
gcloud config set project project-id
gcloud config set compute/zone us-east4-c
gcloud config set compute/region us-east4
```
> Only set the `compute/*` properties if compute API is enabled for the project.

Alternatively, run `gcloud init` and re-initialize the configuration with new settings. This associates the configuration with the account and sets the project.


## Cloud Storage

## Cloud Shell

```bash
gcloud cloud-shell ssh --authorize-session
```
