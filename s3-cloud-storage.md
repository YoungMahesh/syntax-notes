# [s3](simple storage server) object storage

- store and manage data as objects rather than as files or blocks
- each object is self contained and includes metadata that describes the object's content and attributes
- objects can accessed directly through a unique identifier rather than through a file system hierarchy, which can improve performance

- data security

  - securing access to data: applying permissions, object and bucket settings, access control, policy conditions
  - protecting data: encrypting data, data recovery, replication, versioning

- access
  - resource-based security: bucket policies, object ACLs, bucket ACLs, bucket ownership
  - user-based security: IAM policies

## buckets

- on s3 storage we store each file in a bucket, each bucket will have globally unique name; once created, you cannot change the bucket name
- objects and buckets can be made public
- we can either make whole bucket public or private, s3 doesn't support block public access settings on a per-object basis
- since january 2023, AWS made SSE-S3 the default encryption method for all new object uploads to Amazon S3.
  This change means that any new data uploaded is automatically encrypted without any additional cost or performance impact,
  further enhancing security measures

### access keys + bucket policy

- Access Keys
  - Attached to IAM users, groups, or roles; Controls what a user can do in AWS
  - Do not directly control access to individual objects; IAM policies typically apply broadly based on the user's role or group
  - long-term credentials for an IAM user or the AWS account root user, used to sign programmatic requests
    to the AWS CLI or AWS API. They consist of an access key ID and a secret access key.
- Bucket policies
  - Attached to an S3 bucket; Controls who can access a bucket
  - Can specify permissions at the object level by including the object as the resource in the bucket policy
  - are resource-based policies that you attach to an S3 bucket to grant access permissions to the bucket
    and the objects within it. They are written in JSON, using the AWS Identity and Access Management (IAM) policy language.
    Only the bucket owner can associate a policy with a bucket
- Fundamentally, access keys are about who is making the request, while bucket policies are about what they can do with the bucket
  and its contents. They often work together to provide a comprehensive security model for AWS resources.

---

# optional

## access control lists (ACLs)

- for new buckets, ACLs are disabled by default
- AWS recommends disabling ACLs to simplify permissions management and using policies to control access to all objects in your bucket,
  irrespective of who uploaded them
- ACLs allow you to set granular permissions for different AWS accounts or groups, granting or revoking access at the object level
