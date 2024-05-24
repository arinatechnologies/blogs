---
title:'Multi-region key (MRK) BYOK (bring your own key) to AWS KMS '
date:'2024-05-28'
image:'https://drive.google.com/file/d/1lRZ2mTwoUlKagenqb5Q_bbBH4SeGX1Mk/view?usp=sharing'
tags:['BYOK',AWS','KMS','CloudHSM','EC2','Public key,'Symmetric key']
---
### Introduction

AWS Key Management Service (KMS) provides a secure, centralized platform for managing cryptographic keys. Multi-Region keys in AWS KMS allow you to use the same keys across multiple AWS Regions, making it easier to manage encrypted data and ensuring business continuity. In this guide, we'll explore how to set up and use Multi-Region

BYOK (Bring Your Own Key) in AWS KMS.<Video id="0z3rAhxowHY" title=
Multi-region key (MRK) BYOK (bring your own key) to AWS KMS"/>

**Step 1: Setting Up Your Environment**

Before you start, ensure you have an AWS account with the necessary permissions, AWS CLI installed, and familiarity with AWS regions and KMS concepts.

**Creating an Empty Directory on EC2 Instance:**

Start by creating a new directory on your EC2 instance where you'll manage your keys:

```bash
mkdir /opt/vb-hsm/hsm-21
cd /opt/vb-hsm/hsm-21
```

**Step 2: Creating a Multi-Region KMS Key**

Generate a new KMS key with no key material associated, indicating it's external and multi-region:

```bash
aws kms create-key --origin EXTERNAL --region us-east-1 --multi-region
```

You'll receive an output similar to this:

```json
{
    "KeyMetadata": {
        "AWSAccountId": "735360830536",
        "KeyId": "mrk-d58582b2563a40ef893d9181052130db",
        "Arn": "arn:aws:kms:us-east-1:735360830536:key/mrk-d58582b2563a40ef893d9181052130db",
        ...
        "MultiRegion": true,
        ...
    }
}
```

Make sure to note down the `KeyId` as it will be used later.

**Step 3: Preparing for Key Import**

Create an alias for easier reference to your key:

```bash
aws kms create-alias --alias-name alias/byok-mrk --target-key-id mrk-d58582b2563a40ef893d9181052130db
```

Retrieve the parameters needed for importing your key:

```bash
aws kms get-parameters-for-import --key-id mrk-d58582b2563a40ef893d9181052130db --wrapping-algorithm RSAES_OAEP_SHA_256 --wrapping-key-spec RSA_2048 --region us-east-1 > ./WrappingParameters.json
```

Extract the import token and create a public key:

```bash
jq -r '.ImportToken' ./WrappingParameters.json > ./ImportToken.b64
echo -e "-----BEGIN PUBLIC KEY-----\n$(jq -r '.PublicKey' ./WrappingParameters.json)\n-----END PUBLIC KEY-----" > ./PublicKey.pem
openssl enc -d -base64 -A -in ./ImportToken.b64 -out ./ImportToken.bin
```

**Step 4: Importing Key Material**

Initialize your Crypto user and import settings:

```bash
export CLOUDHSM_ROLE=crypto-user
export CLOUDHSM_PIN=cu_user1:acord12345
/opt/cloudhsm/bin/cloudhsm-cli key import pem --path ./PublicKey.pem --label wrapping-key-example-21 --key-type-class rsa-public --attributes wrap=true
```

**Step 5: Replicating Key to Another Region**

Understand the mechanics of cross-Region replication, crucial for maintaining data consistency across geographical locations. Use AWS KMS’s built-in replication features to copy key material securely between regions, adhering strictly to AWS security protocols.

### Conclusion

Using AWS KMS Multi-Region keys with BYOK configurations adds a layer of flexibility and security to your cloud infrastructure, enabling seamless data encryption and decryption across different AWS Regions. By carefully following these steps and maintaining rigorous security standards, you can ensure the safety of your key material and uphold compliance across your organization.