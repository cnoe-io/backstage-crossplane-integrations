# Backstage Crossplane Integrations

## 🏃‍♀️ Prerequisites

1. We might need a container engines such as `Docker Desktop`, `Podman` to run backstage crossplane integrations locally. Please check [this](https://github.com/cnoe-io/idpbuilder?tab=readme-ov-file#prerequisites) documentation to setup your container engine.

2. Download and install [idpbuilder](https://github.com/cnoe-io/idpbuilder?tab=readme-ov-file#download-and-install-the-idpbuilder) for running backstage crossplane integrations.

## 🌟 Implementation walkthrough
 
1. Clone the [cnoe-io/stacks](https://github.com/cnoe-io/stacks) repository
2. Update [the credentials secret file](crossplane-providers/provider-secret.yaml)
3. Next, in the `idpbuilder` folder, navigate to `./ref-implementation/backstage/manifests/install.yaml` and add the following lines for catalog location at line 171 in backstage config to deploy crossplane backstage templates to backstage:

```yaml
        - type: url
          target: https://github.com/cnoe-io/backstage-crossplane-integrations/blob/main/backstage-crossplane-templates/catalog-info.yaml
          rules:
            - allow: [User, Group]
```
4. `idpBuilder` is extensible to launch custom Crossplane patterns using package extensions.  Please use the below command to deploy an IDP reference implementation with an Argo application for preparing up the setup for crossplane integrations:

```bash
idpbuilder create \
  --use-path-routing \
  --package-dir [path-to-stacks-repo]/ref-implementation \
  --package-dir [path-to-stacks-repo]/crossplane-integrations \
  --package-dir https://github.com/cnoe-io/backstage-crossplane-integrations//integration
```
## What is installed?

- Crossplane Runtime
- AWS providers
- Basic Compositions

5. Finally, please make sure to setup Postgres credentials as a kubernetes secret on `crossplane-system` namespace with name of the secret to be `postgres-root-user-password` for the RDS Database.
