# Add a new hub

## Step 0: Open a GitHub issue to track hub creation

In order to track progress on deploying the new hub, [open a GitHub issue in the
`pilot-hubs/` repository](https://github.com/2i2c-org/pilot-hubs/issues/new/choose).
Make sure to choose the `new-hub` issue template.

## Step 1: Decide where the hub will live

Hubs need to exist on a kubernetes cluster, so we will need to
decide which cluster it should live in.

1. For standalone hubs where we are being paid or the client has cloud
   credits, we will need to create a cluster specifically for this hub
   before deploying it. As a general rule, we will create one cluster per
   billing account.
2. For hubs we are running for free, they will go into the `pilot-hubs`
   cluster.
3. Hubs run with the support of [cloudbank](https://www.cloudbank.org/),
   will go in the `cloudbank` cluster.

Each cluster has configuration + list of hubs it supports under
`config/hubs/<cluster-name>.cluster.yaml`.

## Step 2: Decide the template used for the hub

Hub templates map on to different major use-cases that we aim to enable.
Ask the hub user(s) about their needs and expected usage, and use their answer to select a [hub template](../../topic/hub-templates.md).
to be used.

## Step 3: Decide the authentication provider to be used

In consultation with the users, decide
[which authentication provider](https://pilot.2i2c.org/en/latest/admin/configuration/login.html#authentication)
the hub should use.


```{note}
While this can be changed later, it's a messy
process - so we should try to get this right the first time
```

## Step 4: Decide the user image for the hub

The default user image is present in this repository (`images/user`),
and is geared towards simple datascience classes - with both R and
Python. Determine if this is acceptable - if not, you need to
make a [custom image](../configure/update-env.md) for them.

## Step 5: Add hub config

Add an entry for the hub in `config/hubs/<cluster-name>.cluster.yaml` for this hub.
The following docs might be helpful:

1. [Configuration Structure](../../topic/config.md)
2. [JSON Schema for `.cluster.yaml` file](https://github.com/2i2c-org/pilot-hubs/blob/master/config/hubs/schema.yaml)
3. [Zero to JupyterHub on k8s Docs](https://zero-to-jupyterhub.readthedocs.io/en/latest/), since ultimately
   that is what we are configuring.

## Step 6: Deploy manually and test

[Deploy the hub manually](./manual-deploy.md) and test to make sure it works
the way you would like it to. This might take some iteration and multiple
tries. Specifically test that at least the following work ok:

1. Authentication provider
2. Admin user access
3. User image in use (check the `JUPYTER_IMAGE` environment variable in a spawned server)
4. Default user interface when the user logs in (lab, notebook, rstudio, retrolab, etc)
5. Home page display configuration
6. (Optionally) `dask-gateway` functionality
7. (Optionally) Access to any cloud resources (like storage buckets, etc)
   granted to the hub users

## Step 7: Make a PR

Make a PR with your changes, referencing the issue for creation of the hub. Seek
review from someone else, and get this merged!

Getting this merged will mark the hub as being deployed.

Finally, notify the users of the hub that it is now deployed, and ask that they test it out to make sure it works as expected.
If they are happy with the result, then close the issue for deploying the new hub.
Congratulations, you are done! 🎉
