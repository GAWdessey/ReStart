# Lab: Managing Storage in AWS

## Lab Overview
AWS provides multiple ways to manage data on **Amazon Elastic Block Store (Amazon EBS)** volumes. In this lab, you will use the **AWS Command Line Interface (AWS CLI)** to create snapshots of an EBS volume and configure a scheduler to run Python scripts to delete older snapshots.

You will also be challenged to sync the contents of a directory on an EBS volume to an **Amazon Simple Storage Service (Amazon S3)** bucket using the S3 sync command.



## Objectives
By the end of this lab, you will be able to:
* Create and maintain snapshots for Amazon EC2 instances.
* Use `aws s3 sync` to copy files from an EBS volume to an S3 bucket.
* Use Amazon S3 versioning to retrieve deleted files

---

## Lab Environment & Prerequisites
* **VPC:** A Virtual Private Cloud with a public subnet.
* **EC2 Instances:** Two instances are pre-provisioned:
    * **Command Host:** Used to administer resources.
    * **Processor:** The instance containing the EBS volume we will manage.

### Accessing the Lab
1.  Launch the lab environment (Wait for "Lab status: ready").
2.  Open the **AWS Management Console** in a new browser tab.
3.  Arrange your windows so you can see these instructions and the AWS Console side-by-side.

---

## Task 1: Creating and Configuring Resources

### Task 1.1: Create an S3 Bucket
1.  In the AWS Management Console, search for and open **S3**.
2.  Choose **Create bucket**.
3.  Configure the following:
    * **Bucket name:** Enter a unique name (e.g., `s3-bucket-yourname-123`).
    * **Region:** Leave as default.
4.  Choose **Create bucket**.
    > *Note the bucket name. It will be referred to as `S3-BUCKET-NAME` throughout the lab.*

### Task 1.2: Attach Instance Profile to "Processor"
We must give the "Processor" instance permission to access S3 and EBS.
1.  Open the **EC2 Management Console**.
2.  In the navigation pane, choose **Instances**.
3.  Select the **Processor** instance.
4.  Choose **Actions > Security > Modify IAM role**.
5.  Select **S3BucketAccess** from the dropdown list.
6.  Choose **Update IAM role**.

---

## Task 2: Taking Snapshots of Your Instance

### Task 2.1: Connecting to the "Command Host"
1.  In the EC2 Console, select the **Command Host** instance.
2.  Choose **Connect**.
3.  Select **EC2 Instance Connect** and choose **Connect**.
    * A terminal window will open in your browser. You will use this terminal for all CLI commands.

### Task 2.2: Taking an Initial Snapshot
Run the following commands in your terminal to identify your resources and create a snapshot.

**1. Get the Volume ID:**
```bash
aws ec2 describe-instances --filter 'Name=tag:Name,Values=Processor' --query 'Reservations[0].Instances[0].BlockDeviceMappings[0].Ebs.{VolumeId:VolumeId}'
Copy the output (e.g., vol-1234abcd). You will use this as VOLUME-ID.

2. Get the Instance ID:

Bash

aws ec2 describe-instances --filters 'Name=tag:Name,Values=Processor' --query 'Reservations[0].Instances[0].InstanceId'
Copy the output (e.g., i-0b069...). You will use this as INSTANCE-ID.

3. Stop the "Processor" Instance: Replace INSTANCE-ID with your actual ID:

Bash

aws ec2 stop-instances --instance-ids INSTANCE-ID
4. Verify the Instance has Stopped:

Bash

aws ec2 wait instance-stopped --instance-id INSTANCE-ID
(The command prompt will return once the instance has fully stopped).

5. Create the Snapshot: Replace VOLUME-ID with your actual ID:

Bash

aws ec2 create-snapshot --volume-id VOLUME-ID
Copy the SnapshotId from the output (e.g., snap-064...) if needed.

6. Wait for Snapshot Completion:

Bash

aws ec2 wait snapshot-completed --snapshot-id SNAPSHOT-ID
7. Restart the Instance:

Bash

aws ec2 start-instances --instance-ids INSTANCE-ID
Task 2.3: Scheduling Automated Snapshots (Cron)
We will use a cron job to take a snapshot every minute.

1. Create the Cron Job file: Replace VOLUME-ID with your actual ID:

Bash

echo "* * * * * aws ec2 create-snapshot --volume-id VOLUME-ID 2>&1 >> /tmp/cronlog" > cronjob
2. Activate the Cron Job:

Bash

crontab cronjob
Wait 2-3 minutes for the job to run a few times.

3. Verify New Snapshots:

Bash

aws ec2 describe-snapshots --filters "Name=volume-id,Values=VOLUME-ID"
(Run this command multiple times to see the list of snapshots growing).

Task 2.4: Retaining Only the Last Two Snapshots
We will use a Python script to clean up old snapshots, keeping only the 2 most recent ones.

1. Stop the Cron Job:

Bash

crontab -r
2. Review the Python Script:

Bash

more /home/ec2-user/snapshotter_v2.py
(This script lists snapshots, sorts them by date, and deletes all but the newest two).

3. Check Current Snapshot Count:

Bash

aws ec2 describe-snapshots --filters "Name=volume-id, Values=VOLUME-ID" --query 'Snapshots[*].SnapshotId'
4. Run the Cleanup Script:

Bash

python3.8 snapshotter_v2.py
(You should see output listing the deleted snapshots).

5. Verify Only 2 Snapshots Remain:

Bash

aws ec2 describe-snapshots --filters "Name=volume-id, Values=VOLUME-ID" --query 'Snapshots[*].SnapshotId'
Task 3: Challenge - Synchronize Files with Amazon S3
In this challenge, you will sync files from your EC2 instance to S3 and use Versioning to recover a deleted file.

Task 3.1: Download Sample Files
1. Download the files:

Bash

wget [https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-RSJAWS-3-124627/183-lab-JAWS-managing-storage/s3/files.zip](https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-RSJAWS-3-124627/183-lab-JAWS-managing-storage/s3/files.zip)
2. Unzip the files:

Bash

unzip files.zip
Task 3.2: Syncing and Versioning (The Solution)
1. Enable S3 Versioning: Replace S3-BUCKET-NAME with your bucket name:

Bash

aws s3api put-bucket-versioning --bucket S3-BUCKET-NAME --versioning-configuration Status=Enabled
2. Sync Local Files to S3:

Bash

aws s3 sync files s3://S3-BUCKET-NAME/files/
3. Delete a Local File:

Bash

rm files/file1.txt
4. Sync Deletion to S3: Use the --delete flag to remove the file from the S3 current version:

Bash

aws s3 sync files s3://S3-BUCKET-NAME/files/ --delete
(Output should show delete: s3://.../file1.txt)

5. Locate the Previous Version ID: To restore the file, we first find its Version ID:

Bash

aws s3api list-object-versions --bucket S3-BUCKET-NAME --prefix files/file1.txt
Look for the VersionId in the Versions block (not the DeleteMarker).

6. Restore the File: Replace VERSION-ID with the long string found in the previous step:

Bash

aws s3api get-object --bucket S3-BUCKET-NAME --key files/file1.txt --version-id VERSION-ID files/file1.txt
7. Verify Restoration:

Bash

ls files
(You should see file1.txt is back).

8. Re-sync to S3: Push the restored file back to the bucket as the current version:

Bash

aws s3 sync files s3://S3-BUCKET-NAME/files/
Conclusion
You have successfully:

Created manual and automated EBS snapshots.

Implemented a retention policy using a Python script.

synchronized files to S3 and recovered deleted data using Versioning.
