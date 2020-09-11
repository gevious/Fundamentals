## Introduction
In this lesson you will learn how to set up a free Amazon Web Services (or AWS) account, and successfully run commands from the AWS Command Line Interface.

## AWS account Setup
Setting up an AWS account is straight forward. AWS has a free tier, which you'll use today. Just make sure you spend time up-front to understand the pricing for resources you will create in the future.

To create an account, go to the [AWS home page](https://aws.amazon.com) and click on 'Create an AWS account' in the top right corner. Follow the work flow, and enter your personal details, and your credit card details. 

The activation process can take a few minutes. You'll know you're done when you get an email stating the account is ready for use.


## Creating an IAM user
Next up, you will create an IAM user. IAM stands for Identity & Access Management and is AWS' access management tool. Its best practice not to use your main user account (aka the root user account) for day-to-day use. To follow best practice, you will create an administrative IAM user with full access to the account, then create another IAM account with limited permissions. The admin account is useful, since you don't need to be concerned about permissions issues. Just be careful when using admin credentials, since you can use it to accidentally delete resources you created, or create resources that could cost you a lot of money.

Go to the [IAM console](https://console.aws.amazon.com/iam/home?#/users) and click on 'Users'. Create a new user, and give them programmatic access.
Next, click on 'attach existing policies directly' and choose 'AdministratorAccess'. Click through the rest of the work-flow until you see the access and secret keys.  Copy the access and secret keys to a file so you can access them later.

## Installing the AWS CLI
Now its time to set up the AWS CLI. The [AWS Documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html) has the installation details. The commands you run will download a zip file with the files you need, uncompress the zip file, and then run an install script. If you're running on a Linux system like me, it'll take you less than a minute to get set up.

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Now type `aws` in your terminal, and you should see a set of usage instructions. If you see something like `command not found: aws`, the installation failed. Try restarting your terminal, or go back to the installation instructions and try again.

## Linking an IAM user to the AWS CLI
To run the first command with the AWS CLI, you need to first configure it with an IAM user, and select a default region to work in. You can use the default region (us-east-1), but you'll have a better experience if you use the region closest to your geographic location. Run the `aws configure` command, and paste the access and secret keys into the terminal when asked. For the output format, I like using table or json.

```
aws configure
```

Do you see what just happened? You set up the CLI to use a default region, and a default set of credentials. If ever you're wondering what the credentials are, you can see them in the `~/.aws/credentials` file. You can also change the default region or output format in the `~/.aws/config` file.

Lets make sure this works.

```
aws s3api list-buckets
```
You'll see that the command returns successfully, even though you have no S3 buckets created. Lets create one, list it, and then remove it again.

```
aws s3api create-bucket --bucket my-gevious-test-bucket
aws s3api list-buckets
aws s3api delete-bucket --bucket my-gevious-test-bucket
```

S3 is a powerful tool, but it has some drawbacks. For example, behind the scenes S3 replicates data to minimize the chance of data getting corrupted or lost. The drawback is that this takes a while (its called eventual consistency). You'll find that the `delete-bucket` command may not delete the bucket immediately. This means that if you immediately run the set of commands above again, they mail fail. I typically avoid deleting a bucket if I need to use the same bucket name again.

## AWS CLI Profiles
You can set up multiple profiles for your CLI to use. We already set up the default one. Let's create a new IAM user without permissions to create resources. That way you will have an admin profile, and a default, read-only profile.

Set up a new IAM user, that cannot create AWS resources. Before you used the web console to do this, but you can now use the CLI to create that account. Once created, attach a policy that gives the account read-only access to all S3 resources.

```
aws iam create-user --user-name ro-all
aws iam attach-user-policy --user-name ro-all --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
aws iam create-access-key --user-name ro-all
```

Now edit the `~/.aws/credentials` file. Copy paste the default profile into the file. Change the name of the second profile to 'admin'. Next, replace the access and secret keys of the first profile to match the IAM user you just created. Next, do the same with the `~/.aws/config` file, copying the profile, and replacing the name of the second to 'admin'.

Lets test this out:

```
aws s3api create-bucket --bucket my-gevious-test-bucket
aws s3api create-bucket --bucket my-gevious-test-bucket --profile admin
aws s3api list-buckets
aws s3api list-buckets --profile admin
aws s3api delete-bucket --bucket my-gevious-test-bucket
aws s3api delete-bucket --bucket my-gevious-test-bucket --profile admin
```

You will see that the create and delete commands with the default profile failed, because it doesn't have permissions. The second command uses the IAM admin user, which has permissions to create resources. However, the default profile can list the buckets, so you can see that it exists.

# Getting help
If you're ever not sure about what commands exist, you can postfix the word 'help' after a command. For example, you can say `aws help` or `aws s3api help`. In this way you can get help at every level of the command.

# Wrap up
Great! You're done. You can now successfully run commands in the CLI. This is a foundational step in cloud computing. I suggest you read up more about IAM and permissions management, and spend time playing with the AWS commands to see what they do. Starting adding, modifying and deleting data in S3 is a good first step, since you get instant feedback of your actions, and it won't cost you anything.

I am gevious. Happy building!


# Appendix - References
- [Create an AWS Account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)
- [Link to AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-linux.html)
- [AWS CLI Configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
