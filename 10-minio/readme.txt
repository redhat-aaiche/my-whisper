
mkdir aa; cd aa

wget https://github.com/rh-aiservices-bu/fraud-detection/raw/main/setup/setup-s3.yaml

vim setup-s3.yaml

	° Change the Image: from 2024.1 to 2023.2
	
	° To Access the S3 buckets, we don't want to use TLS ( i.e. httpS )
   	  a) Comment the lines of the tls attribute. 
	  b) Change MINIO_HOST variable
	  
	  This has to be done only for the API route, and we can leave the console Route as it is

		==> So in summary, if we want to access the console ( browser ), we will use https
		==> If we want to access the buckets programmatically, we will use http

-->  Create the resources defined in the YAML file :
oc login -u developer -p developer https://api.ocp4.example.com:6443
oc project aa
oc apply -n aa -f setup-s3.yaml

--> Inspect the resources that have been created by the YAML file :
oc get route


--> Access the Minio Server Web Console :
oc get secret/storage-config -o jsonpath="{.data.aws-connection-my-storage}" | base64 -d | jq
oc get secret/storage-config -o jsonpath="{.data.aws-connection-pipeline-artifacts}" | base64 -d | jq

--> Access the MinIO Web Console :
        ° Use the credentials, see above
firefox  https://minio-console-aa.apps.ocp4.example.com &


