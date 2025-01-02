# **Create an IAM User with Limited S3 Access**

This guide outlines the steps to create an IAM user in AWS with restricted access to a specific S3 bucket. The user will have programmatic access to perform operations only on the assigned bucket.

---

## **Prerequisites**
1. AWS Management Console access with sufficient IAM permissions to create users and policies.
2. An existing S3 bucket. If not, create one in the **S3 Console**.

---

## **Steps to Implement**

### 1. **Log into the AWS Management Console**
- Open [AWS Console](https://aws.amazon.com/console/).
- Navigate to the **IAM Console**.

### 2. **Create a New IAM User**
1. In the IAM Console, go to **Users** → **Add Users**.
2. Enter a username, e.g., `s3-limited-user`.
3. Under **Access type**, select **Programmatic access** to generate API keys for CLI/SDK access.
4. Click **Next**.

---

### 3. **Set Permissions**
1. On the **Set Permissions** page, choose **Attach policies directly** → **Create policy**.
2. Select the **JSON** tab and paste the following policy:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "s3:*",
         "Resource": [
           "arn:aws:s3:::your-bucket-name",
           "arn:aws:s3:::your-bucket-name/*"
         ]
       }
     ]
   }
   ```

3. Replace `your-bucket-name` with the name of your S3 bucket.
4. Click **Review Policy**, assign a name (e.g., `S3LimitedAccessPolicy`), and save.
5. Attach the newly created policy to the user.

---

### 4. **Complete User Creation**
1. Review the user details and finish the creation process.
2. Download the **Access Key ID** and **Secret Access Key**. Save them securely as they won’t be shown again.

---

### 5. **Test the User's Access**
#### **Install and Configure AWS CLI**
1. Install the AWS CLI if not already installed. Refer to [AWS CLI installation guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).
2. Configure the AWS CLI with the new user's credentials:
   ```bash
   aws configure
   ```
   - Enter the **Access Key ID** and **Secret Access Key** from the downloaded file.
   - Specify the AWS Region where your S3 bucket resides.
   - Leave the default output format as JSON or select your preference.

#### **Validate Access**
1. Test access to the specific S3 bucket by listing its contents:
   ```bash
   aws s3 ls s3://your-bucket-name
   ```
2. Attempt to access another bucket or perform unauthorized actions to verify the restrictions:
   ```bash
   aws s3 ls s3://another-bucket-name
   ```

---

## **Expected Results**
- The user can only list, read, or perform actions on the specified S3 bucket.
- Access to other buckets or unauthorized actions will be denied.

---

## **Cleanup (Optional)**
- Delete the IAM user and policy if no longer needed to avoid unnecessary costs or security risks.
- Navigate to the IAM Console → Users → Delete User.





