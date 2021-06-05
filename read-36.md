##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 36 - Cognito

<hr>

# Configure Auth Category

* To start provisioning auth resources in the backend, go to your project directory and execute the command:

`amplify add auth`

* Enter the following when prompted:

```CLI
? Do you want to use the default authentication and security configuration?
    `Default configuration`
? How do you want users to be able to sign in?
    `Username`
? Do you want to configure advanced settings?
    `No, I am done.`
```

* To push your changes to the cloud, execute the command:

`amplify push`

# Install Amplify Libraries

* Add the following dependency to your app‘s `build.gradle` along with others you added above in Prerequisites and click “Sync Now” when prompted:

```java
dependencies {
    implementation 'com.amplifyframework:aws-auth-cognito:1.17.4'
}
```

# Initialize Amplify Auth

* Add the Auth plugin before calling **Amplify.configure**

```java
// Add this line, to include the Auth plugin.
Amplify.addPlugin(new AWSCognitoAuthPlugin());
Amplify.configure(getApplicationContext());
```

# Check the current auth session

* We can now check the current auth session.

* For testing purposes, you can run this from your MainActivity’s `onCreate` method.

```java
Amplify.Auth.fetchAuthSession(
    result -> Log.i("AmplifyQuickstart", result.toString()),
    error -> Log.e("AmplifyQuickstart", error.toString())
);
```

<hr>

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.

###### [resource1](https://docs.amplify.aws/lib/auth/getting-started/q/platform/android)
<!-- ###### [resource2]() -->
<!-- ###### [resource3]() -->
<!-- ###### [resource4]() -->