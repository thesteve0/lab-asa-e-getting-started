= Getting Started with Azure Spring Apps - Enterprise

Here are the steps to get it working with docker.

if you are using these instructions to help deploy a different workshop, replace all instances of `lab-asa-e-getting-started`
with the directory name of your workshop

All the cli commands are run in the directory containing `.git`

1. Make sure you have Carvel ImgPkg https://carvel.dev/imgpkg/docs/v0.34.0/install/[installed] on your machine.
2. Run this command to dowload the latest client

        imgpkg pull -i ghcr.io/vmware-tanzu-labs/educates-client-programs:latest -o educates-client-programs

3. This puts the binary in the following directory `~/educates-client-programs/bin`, put the appropriate binary either in a directory
in your path or add that directory to your path.
4. Now you publish your workshop

    educates docker publish-workshop asa-e-getting-started

5. And then you deploy your workshop

    educates docker workshop deploy -f asa-e-getting-started
+
And this should pop-up the browser and you should be good to go
+
6. Sometimes the changes don't get picked up. Just delete the workshop and go through the steps again

    educates docker workshop delete -f asa-e-getting-started



doc is here for the CLI version of educates
https://docs.educates.dev/workshop-content/building-an-image.html



As a result, do the following:

    educates delete-cluster --all

That will delete cluster created with CLI as well as the image registry.
Ensure not running any workshop in docker itself.

    educates docker workshop list

For each work listed, run:

    educates docker workshop delete -n xxx

where xxx is the name of each successive workshop.
Now for the important step, run:

    docker system prune

or at least:
    docker network prune

as we want to get rid of any old docker networks for the Kubernetes kind cluster or educates.
This time skip deploying Kubernetes, instead try just the following:

    educates admin registry deploy

This will deploy the registry. This will happen as side effect of trying to publish a workshop first time, but lets do it separately.
Now create a new workshop (OPTIONAL  if I have already written the content.

    educates new-workshop sample-workshop

Publish this workshop.

    educates publish-workshop sample-workshop

Now deploy the workshop to docker.

    educates docker workshop deploy -f sample-workshop

If you are running desktop on the same host, this should have side effect of opening browser on the workshop. If for some reason it doesn't, can open the working using:

    educates docker workshop open -f sample-workshop


https://docs.educates.dev/custom-resources/workshop-definition.html#defining-additional-ingress-points

https://docs.educates.dev/workshop-content/workshop-instructions.html#passing-of-environment-variables

https://docs.educates.dev/workshop-content/workshop-instructions.html#clickable-actions-for-the-dashboard

## To run this locally
1. Make sure you get the latest educates cli from Github
2. Change to the top directory of the repo

```shell
cd /home/spousty/git/lab-asa-e-getting-started
```

3. Make sure there are no old workshops hanging around

```shell
educates list-workshops
educates docker workshop delete -f asa-e-getting-started
```
   
5. Publish the workshop content

```shell
educates publish-workshop asa-e-getting-started

```

6. Deploy the workshop to educates

```shell
educates docker workshop deploy -f asa-e-getting-started
```

