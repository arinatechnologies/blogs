---
title: 'Access CodeCommit/Git via AWS Identity Center'
date: '2024-08-12'
image: 'https://raw.githubusercontent.com/arinatechnologies/blogs/08b1e928c54bedd5e7fc4bc3144d978bc3c12cd5/images/codecommit.webp'
tags: ['AWS CodeCommit','Identity Center','AWS SSO','Git Bash','Python Installation','Git Remote Setup','AWS CLI','Visual Studio Code','GitHub Desktop','DevOps Tools','Cloud Development','Version Control','Software Installation','Cloud Computing']
---
# Access CodeCommit/Git via AWS Identity Center

## Pre-requisites

- Ensure your SSO User has CodeCommit access.
- You should have GitBash CLI installed on your machine.
- Able to install Python preferably 3.12 version.
- Able to install other software such as `git-remote-codecommit`.

## Setup AWS Profile
<Video id="yn1ekwAZK3k" title="Access CodeCommit/Git via AWS Identity Center | DevOps Best Practices & Secure Git Integration"/>

The second main feature we want to enable is AWS SSO login from the AWS Command Line Interface (AWS CLI) on our local machine.

```bash
aws configure sso
SSO start URL [None]: https://<sso-name>.awsapps.com/start/#
SSO region [None]:us-east-1
```

You will be redirected to your default browser. Or copy the link provided in your browser and ensure the code provided matches what is shown in CLI.

In case you have access to more than 1 account, when you return to the CLI, you must choose your account.

```bash
There are 2 AWS accounts available to you.
> AdministratorAccess, <email> (<Account1>)
> AdministratorAccess, <email2> (Account2)
```

Choose the account with your CodeCommit repository.

Next, you see the permissions sets available to you in the account you just picked.

You now see the options for the profile you’re creating for these AWS SSO permissions:

```bash
CLI default client Region [None]: us-east-1<ENTER>
CLI default output format [None]: json<ENTER>
CLI profile name [<Account1>-Developer]: Dev-profile<ENTER>
```

Note: In GitBash, if you get an error such as:

```bash
http://aws.amazon.com/cli
http://aws.amazon.com/cli
https://asg-infra.awsapps.com/start/#/console?account_id=735360830536&role_name=AdministratorAccess
```

You can run the command from CMD or another WSL.

## Git Bash Setup

### Python Installation

To install Python on Git Bash, follow these steps:

1. Download Python:
   - Visit the official [Python downloads page](https://www.python.org/downloads/).
   - Choose the latest version of Python for your operating system (Windows) and download the installer.

2. Install Python:
   - Run the Installer:
     - Locate the downloaded installer file and double-click to run it.
   - Customize Installation:
     - Check the box that says "Add Python to PATH". This is crucial as it allows you to use Python from the command line.
   - Click on "Customize installation" for more options if needed.
   - Choose Optional Features:
     - You can leave the default options checked. Click "Next".
   - Advanced Options:
     - Leave the default options or adjust as needed. Click "Install".

3. Verify Python Installation:
   - Open Git Bash:
   - Type the following command and press Enter:
     ```bash
     python --version
     ```
   - You should see the version of Python that you installed.

4. Install `pip`:
   - `pip` usually comes bundled with Python, but if it's not available, you can install it manually.

     ```bash
     curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
     python get-pip.py
     ```

   - Verify `pip` Installation:
     ```bash
     pip --version
     ```

### Installing `git-remote-codecommit`

To install `git-remote-codecommit` in Git Bash:

1. Install with the following code:
   ```bash
   pip install git-remote-codecommit
   ```
2. For some operating systems, you might need to run:
   ```bash
   sudo pip install git-remote-codecommit
   ```

3. Clone the code from one of your repositories:
   ```bash
   git clone codecommit://<profile name>@<CodeCommit repo name>
   ```
   Example:
   ```bash
   git clone codecommit://AdministratorAccess-735360830536@asg-admin
   ```

## Reconnect if Session Expired

If your SSO session expires, follow these steps to reconnect:

1. Run the following command in Git Bash or another WSL:
   ```bash
   aws sso login --sso-session <session name>
   ```

2. If you have forgotten the session name, you can find it in `C:\Users\<UserName>\.aws\config`.

3. Follow the steps where a URL will open and accept as shown.

## GitHub Desktop Setup

1. Ensure `git-remote-codecommit` is installed in Git Bash CLI as described above.
2. Follow the instructions provided to use GitHub Desktop.

## Visual Studio Code Setup

1. Ensure `git-remote-codecommit` is installed in Git Bash CLI as described above.
2. Follow the provided highlights.
```

# Other Blogs
[Azure Messaging: Service Bus, Event Hub & Event Grid](https://www.arinatechnologies.com/posts/az-messaging) <br/>
[How to improve data transfer efficiency in Azure](https://www.arinatechnologies.com/posts/azure-data-transfer) <br/>
[AWS vs Google vs Azure: Decoding the Ultimate Cloud Battle](https://www.arinatechnologies.com/posts/aws-gcp-azure) <br/>
