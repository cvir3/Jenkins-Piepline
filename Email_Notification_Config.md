# Jenkins Email Notification Setup (Gmail SMTP)
- This guide explains how to configure email alerts in Jenkins pipelines using Gmail SMTP, including secure authentication with App Passwords and pipeline integration.

## What is an App Password?
- App Password is a 16-character password generated from your Google account. It allows third-party applications (like Jenkins) to securely send emails without exposing your main password.

## Step 1: Enable 2-Step Verification
Before generating an App Password, enable 2-Step Verification:
1. Go to Google Account Security
2. Navigate to Signing in to Google
3. Click 2-Step Verification
4. Click Get Started
5. Complete setup using:
    - SMS verification, or
    - Authenticator app

## Step 2: Generate App Password
1. Go to Google App Passwords
2. Sign in to your Google account
3. Under Select App → choose Mail
4. Under Select Device → choose Other (Custom Name)
    - Enter: Jenkins SMTP
5. Click Generate
6. Copy the generated 16 character password
7. Use this password in Jenkins credentials

## Step 3: Install Required Plugins
1. Navigate to Manage Jenkins → Plugins
2. Email Extension Plugin (email-ext)

## Step 4: Create Credentials in Jenkins
1. Navigate to:Manage Jenkins → Credentials
2. Click on Add Credentials
3. Select: Username with password
4. Fill in the details:
    - Username: Your Gmail address (e.g., testops@gmail.com)
    - Treat username as secret: Checked
    - Password: Your App Password
    - ID: (Optional) Provide a unique ID (If not provided, Jenkins generates one automatically)
    - Description: Add a meaningful description
5. Click Create

## Step 5: Configure Extended Email Notification
1. Navigate to Manage Jenkins → System
2. Scroll to Extended E-mail Notification
3. Configure the following:
    - SMTP Server: **smtp.gmail.com**
    - SMTP Port: **465**
4. Click Advanced, then
    - Cdentials: Select the credentials created earlier
    - Use SSL: **Checked**
    - Default user e-mail suffix: **@gmail.com**

## Step 6: Configure Email Notification
1. In the same System page, scroll to E-mail Notification
2. Configure:
SMTP Server: **smtp.gmail.com**
Email Suffix: **@gmail.com**
3. Click Advanced, then:
    - Use SMTP Authentication: **Enabled**
    - Username: **testops@gmail.com**
    - Password: App Password (Recommended if 2FA is enabled)
    - Use TLS: **Checked**
    - SMTP Port: **587**
    - Reply-To Address: **testops+jenkins@gmail.com**
    - Charset: **UTF-8**

## Step 7 :Verification
1. Use Test configuration by sending test e-mail in Jenkins.
2. Check inbox/spam folder for confirmation.

> [!NOTE]
> - If you are using Gmail with 2-Step Verification, you must use an App Password, not your actual Gmail password.
> - Ensure the Email Extension Plugin is installed in Jenkins.


```bash
// Email Notification
post {
    always {
	emailext(
		to: 'testops+jenkins@gmail.com,',
		subject: "${env.JOB_NAME} | Build #${env.BUILD_NUMBER} | ${currentBuild.currentResult}",
		mimeType: 'text/html',
		body: """
		<h2>Pipeline Execution Summary</h2>
		<table border="1" cellpadding="5">
		<tr><td><b>Project</b></td><td>${env.JOB_NAME}</td></tr>
		<tr><td><b>Build Number</b></td><td>${env.BUILD_NUMBER}</td></tr>
		<tr><td><b>Status</b></td><td>${currentBuild.currentResult}</td></tr>
		<tr><td><b>Build URL</b></td><td><a href="${env.BUILD_URL}">Open Build</a></td></tr>
		</table>
		""",
		attachmentsPattern: '**/Extent/LatestReport.html'
	)
    }
}
```