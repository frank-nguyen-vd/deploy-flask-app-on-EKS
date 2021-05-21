# AWS COMMAND LINE CHEATSHEET

_FOR FULLSTACK DEVELOPER NANO DEGREE_

- Display the folder that contains the symlink to the aws cli tool

```bash
which aws
```

- Display AWS CLI version

```bash
aws --version
```

- Create a new profile

```bash
aws configure --profile new_name
```

> :bulb: Moving forward, you can use --profile <profile-name> option with any AWS command. This will resolve the conflict if you have multiple profiles set up locally.

- Let the system know that your sensitive information is residing in the .aws folder

```bash
export AWS_CONFIG_FILE=~/.aws/config
export AWS_SHARED_CREDENTIALS_FILE=~/.aws/credentials
```

- View the current configuration

```bash
aws configure list --profile profile_name
```

- Find AWS account ID

```bash
aws sts get-caller-identity
```

- Create an IAM role

```bash
aws iam create-role --role-name UdacityFlaskDeployCBKubectlRole --assume-role-policy-document file://trust.json --output text --query 'Role.Arn'
```

Where  
`--role-name` allows you to give the role a name of your choice  
`--assume-role-policy-document` provide the path of the trust relationship file  
`--output` defines the output format  
`--query` decides what information to query and display on the terminal. `Role.Arn` means we want to view the ARN of the new role. The ARN stands for Amazon Resource Name, which uniquely identifies AWS resources

- Attach a Policy to the IAM role

```bash
aws iam put-role-policy --role-name UdacityFlaskDeployCBKubectlRole --policy-name eks-describe --policy-document file://iam-role-policy.json
```

Where  
`--role-name` takes the name of the role to associate the new policy with  
`--policy-name` accepts the name of the policy document  
`--policy-document` provides the path of the policy file

- Create a bucket

```bash
aws s3api  create-bucket --bucket my-unique-bucket --acl public-read-write --region us-east-1
```

- Upload a sample file to your bucket

```bash
aws s3api put-object --bucket my-unique-bucket --key sample.html --body sample.html
```

- Update Kubernetes configuration

```bash
aws eks update-kubeconfig --name <cluster name>
```

- [List of s3api commands](https://docs.aws.amazon.com/cli/latest/reference/s3api/index.html#available-commands)

\
\
\
&nbsp;

# USE EKSCTL TO MANAGE KUBERNETES CLUSTER

1. Create a basic cluster

```bash
eksctl create cluster
```

A cluster will be created with default parameters, such as

- An auto-generated name
- Two `m5.large` worker nodes. Recall that the worker nodes are the virtual machines, and the `m5.large` type defines that each VM will have 2vCPUs, 8GiB memory, and up to 10Gbps network bandwidth.
- Use the Linux AMIs as the underlying machine image
- Your default region
- A dedicated VPC

However, parameters can be customized, for example:

```bash
eksctl create cluster --name myCluster --nodes=4
```

2. Create an advanced cluster
   If you want to specify a detailed configuration, then you may have to write all theconfiguration in a separate YAML file, and pass it along with the command:

```bash
eksctl create cluster --config-file=<path>
```

3. List the details
   List the details about the existing cluster(s)

```bash
eksctl get cluster [--name=<name>][--region=<region>]
```

4. Delete a cluster

```bash
eksctl delete cluster --name=<name> [--region=<region>]
```

\
\
\
\
&nbsp;

# KUBERNETES RBAC SYSTEM

1. Get the current ConfigMap and save it to a file

```bash
kubectl get -n kube-system configmap/aws-auth -o yaml > aws-auth-patch.yml
```
