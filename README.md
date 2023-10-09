# SAML to AWS STS Keys Conversion
Google Chrome Extension, which converts a SAML 2.0 assertion to AWS STS Keys (temporary credentials). Just log in to the AWS Web Management Console using your SAML IDP, and the Chrome Extension will fetch the SAML Assertion from the HTTP request. The SAML Assertion is then used to call the assumeRoleWithSAML API to create the temporary credentials. (AccessKeyId, SecretAccessKey and SessionToken).



# Table of Contents
* [Why this Chrome Extension?](#why)
* [Installation](#Installation)
* [Frequently Asked Question](#faq)

## <a name="why"></a>Why this Chrome Extension?
If you don't have any user administration setup within AWS Identity & Access Management (IAM) but instead rely on your corporate user directory, i.e. Microsoft Active Directory. Your company uses a SAML 2.0 Identity Provider (IDP) to log in to the AWS Web Management Console (Single Sign On).
Then this Chrome Extension is for you!

You run into trouble as soon as you want to execute some fancy scripts from your computer, which call the AWS API. When sending a request to the AWS API, you need credentials, meaning AccessKey and SecretKey. You can quickly generate these keys for each user in AWS IAM. However, since you don't have any users in AWS IAM and don't want to create users just for the sake of having an AccessKey and SecretKey, you are screwed. But there is a way to get temporary credentials specifically for your corporate identity.

The Security Token Service (STS) from AWS provides an API action assumeRoleWithSAML. Using the SAML Assertion given by your IDP, the Chrome Extension will call this API action to fetch temporary credentials. (AccessKeyId, SecretAccessKey and SessionToken). This way, there is no need to create some anonymous user in AWS IAM used for executing scripts. This would be an absolute security nightmare since it is impossible to audit who did what. This Chrome Extension, however, will make it super easy for you to use your corporate identity for executing scripts calling AWS API.

## <a name="Installation"></a>Installation
1. Clone this repository
2. Open Chrome and go to `chrome://extensions/`
3. Enable Developer Mode
4. Click on "Load unpacked extension..."
5. Select the folder where you cloned this repository
6. symlink your aws credentials file to your download locations credentials file
  - `ln -s ~/Downloads/credentials ~/.aws/credentials`
7. Enjoy!

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
