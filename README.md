# SAML to AWS STS Keys Conversion
Google Chrome Extension, which converts a SAML 2.0 assertion to AWS STS Keys (temporary credentials). Just log in to the AWS Web Management Console using your SAML IDP, and the Chrome Extension will fetch the SAML Assertion from the HTTP request. The SAML Assertion is then used to call the assumeRoleWithSAML API to create the temporary credentials. (AccessKeyId, SecretAccessKey and SessionToken).

This extension can only download those credentials to Chrome's download folder, but if you symlink that file to you `.aws/credentials` as described below in the [Instructions](#Instructions), then everytime you log into AWS in your browser, your AWS CLI keys will be updated.

All credit to original author: https://github.com/prolane/samltoawsstskeys. This was tweaked slightly and repackaged to support AWS GovCloud.

## <a name="Installation"></a>Installation
1. [Add extension from Chrome Extension store](https://chrome.google.com/webstore/detail/saml-to-aws-sts-keys-conv/djjiojdghhcbkdfhjmmmkbgmoachckjf)
2. symlink your aws credentials file to your download locations credentials file
  - `ln -s ~/Downloads/credentials ~/.aws/credentials`


##### Or add extension from source code
1. Clone this repository
2. Open Chrome and go to `chrome://extensions/`
3. Enable Developer Mode
4. Click on "Load unpacked extension..."
5. Select the folder where you cloned this repository
6. symlink your aws credentials file to your download locations credentials file
  - `ln -s ~/Downloads/credentials ~/.aws/credentials`
7. Enjoy!


### Troubleshooting
If your credentials are being downloaded as expected, yet the CLI isn't picking up the credentials the following steps may help:
- List AWS CLI profiles:
  `aws configure list-profiles`
- List the current AWS CLI configuration data:
  `aws configure list`
  - The last 4 characters of `access_key` and `secret_key` should match your `aws_access_key_id` and `aws_secret_access_key` in the credentials file
  - If `region` is not set, AWS CLI commands will fail saying __An error occurred (InvalidToken) when calling the <> operation: The provided token is malformed or otherwise invalid.__
- Set region for current profile:
  `aws configure`
  - You can just press `ENTER` on the access key and secret key, both should be pre-populated
  - The defualt region name should show: `[None]`. Change this to your default region: __us-gov-west-1__
  - Press `ENTER` to leave the default output format as `[None]`

## <a name="faq"></a>FAQ: Frequently Asked Question
1. How to check for errors in the extension?
    * Go to the options page of the extension
    * Set 'Enable DEBUG logs' to 'yes' and hit the 'Save' button below
    * Go to the Chrome Extensions page and look for the 'SAML to AWS STS Keys Conversion' extension
    * Click on the link where it says 'Inspect views'
    * A new Chrome DevTools window should pop up, with the 'Console' tab already selected. This is where you'll be able to view all logs.
    * Perform a new login and check the logs to see if there are any errors.

2. Why can I not save the credentials file somewhere else?
With security in mind Google has limited the Chrome browser to only read and write to the Chrome Downloads directory. This way none of your Chrome extension will be able to steal data from your computer.

3. How long are the credentials valid?
AWS calls this 'session duration'. The default session duration is 1 hour. The maximum session duration is configured in AWS IAM as an attribute of the IAM Role. Your IDP might be configured to pass along an additional SAML claim which requests to apply a custom session duration. This value can be configured to be higher than the default of 1 hour. However, this can never be higher than the configured maximum session duration on the IAM Role as this will result in an error.

4. Create a symlink to your .aws directory
TODO
