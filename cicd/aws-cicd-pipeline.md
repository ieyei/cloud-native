# AWS CICD Pipeline



## Pushing a Helm chart

https://docs.aws.amazon.com/AmazonECR/latest/userguide/push-oci-artifact.html

1. Install the Helm Client version 3. For more information, see [Installing Helm](https://helm.sh/docs/intro/install/).

2. Enable OCI support in the Helm 3 client.

   ```
   export HELM_EXPERIMENTAL_OCI=1
   ```

3. Create a repository to store your Helm chart.

   ```
   aws ecr create-repository \
        --repository-name artifact-test \
        --region us-west-2
   ```

4. Authenticate your Helm client to the Amazon ECR registry.

   ```
   aws ecr get-login-password \
        --region us-west-2 | helm registry login \
        --username AWS \
        --password-stdin 405554792066.dkr.ecr.us-west-2.amazonaws.com
   ```

5. Save the chart locally and create an alias for the chart with your registry URI.

   ```
   #nginx chart example.
   cd nginx
   helm chart save . mynginx
   helm chart save . 405554792066.dkr.ecr.us-west-2.amazonaws.com/artifact-test:mynginx
   ```

6. Identify the Helm chart to push.

   ```
   $ helm chart list
   REF                                                         	NAME 	VERSION	DIGEST 	SIZE    	CREATED       
   405554792066.dkr.ecr.us-west-2.amazonaws.com/artifact-tes...	nginx	9.1.0  	fc95d90	33.6 KiB	19 seconds    
   mynginx:9.1.0                                               	nginx	9.1.0  	fc95d90	33.6 KiB	About a minute
   
   ```

7. Push the Helm chart using the **helm chart push** command:

   ```
   helm chart push 405554792066.dkr.ecr.us-west-2.amazonaws.com/artifact-test:mynginx
   ```

8. Describe your Helm chart.

   ```
   aws ecr describe-images \
        --repository-name artifact-test \
        --region us-west-2
   ```

   Output:

   ```
   imageDetails:
   - artifactMediaType: application/vnd.cncf.helm.config.v1+json
     imageDigest: sha256:fc95d905268e7ed821e6f2da7e344179507d44fedbb2cfe6a23e8c1627310d6c
     imageManifestMediaType: application/vnd.oci.image.manifest.v1+json
     imagePushedAt: '2021-06-10T10:11:12+09:00'
     imageSizeInBytes: 35086
     imageTags:
     - mynginx
     registryId: '405554792066'
     repositoryName: artifact-test
   ```

   













