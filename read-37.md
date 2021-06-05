##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 37 - S3

<hr>

# What is Amazon S3?

Amazon Simple Storage Service (Amazon S3) is storage for the Internet. It is designed to make web-scale computing easier.

Amazon S3 has a simple web services interface that you can use to store and retrieve any amount of data, at any time, from anywhere on the web. 

<hr>

# Provision backend storage

* To start provisioning storage resources in the backend, go to your project directory and execute the command:

`amplify add storage`

* Enter the following when prompted:

```CLI
? Please select from one of the below mentioned services:
    `Content (Images, audio, video, etc.)`
? You need to add auth (Amazon Cognito) to your project in order to add storage for user files. Do you want to add auth now?
    `Yes`
? Do you want to use the default authentication and security configuration?
    `Default configuration`
? How do you want users to be able to sign in?
    `Username`
? Do you want to configure advanced settings?
    `No, I am done.`
? Please provide a friendly name for your resource that will be used to label this category in the project:
    `S3friendlyName`
? Please provide bucket name:
    `storagebucketname`
? Who should have access:
    `Auth and guest users`
? What kind of access do you want for Authenticated users?
    `create/update, read, delete`
? What kind of access do you want for Guest users?
    `create/update, read, delete`
? Do you want to add a Lambda Trigger for your S3 Bucket?
    `No`
```

* To push your changes to the cloud, execute the command:

`amplify push`

# Install Amplify Libraries

* Expand Gradle Scripts, open build.gradle (Module: app).

* Add these libraries into the dependencies block:

```java
dependencies {
    implementation 'com.amplifyframework:aws-storage-s3:1.17.4'
    implementation 'com.amplifyframework:aws-auth-cognito:1.17.4'
}
```

# Initialize Amplify Storage

* To initialize the Amplify Auth and Storage categories you call `Amplify.addPlugin()` method for each category. To complete initialization call `Amplify.configure()`.

* Add the following code to your `onCreate()` method in your application class:

```java
Amplify.addPlugin(new AWSCognitoAuthPlugin());
Amplify.addPlugin(new AWSS3StoragePlugin());
```

# Uploading data to your bucket

* To upload to S3 from a data object, specify the key and the data object to be uploaded.

```java
private void uploadFile() {
    File exampleFile = new File(getApplicationContext().getFilesDir(), "ExampleKey");

    try {
        BufferedWriter writer = new BufferedWriter(new FileWriter(exampleFile));
        writer.append("Example file contents");
        writer.close();
    } catch (Exception exception) {
        Log.e("MyAmplifyApp", "Upload failed", exception);
    }

    Amplify.Storage.uploadFile(
            "ExampleKey",
            exampleFile,
            result -> Log.i("MyAmplifyApp", "Successfully uploaded: " + result.getKey()),
            storageFailure -> Log.e("MyAmplifyApp", "Upload failed", storageFailure)
    );
}

```

* Upon successfully executing this code, you should see a new folder in your bucket, called `public`. It should contain a file called `ExampleKey`, whose contents is `Example file contents`.

<hr>

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.

###### [resource1](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
###### [resource2](https://docs.amplify.aws/lib/storage/getting-started/q/platform/android)
<!-- ###### [resource3]() -->
<!-- ###### [resource4]() -->