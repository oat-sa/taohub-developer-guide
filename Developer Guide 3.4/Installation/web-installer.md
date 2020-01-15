# Web Installer

> Once TAO has been installed on your server via composer (or the Windows Installer), you can either install TAO via the command-line as already shown or through the web installer as shown here.

To utilize the web installer, you will go to the hostname or IP of your server and to the location you configured in Apache. If you followed the instructions previously provided, you point your browser to the following URL or adjust as needed per your configuration:

```
http://<hostname or IP>/tao
```

On this page, you will see the following screen:

![initial installation](../resources/installation/step-01.png)

This screen will let you know if there are any missing requirements. If everything is correct you will only see **Your web server meeta TAO requirements** followed by a **green** check. If there are permission issues or missing requirements, you will see the **error** proceeded by a **red** line. Or as shown here, you may see a **warning** proceeded by a **yellow** line. It is important to note you can proceed with a warning but not with an error.

Once any errors and optionally any warnings are fixed, you will click on *next* to proceed to the next screen where you will configure your server.

![server setup](../resources/installation/step-02.png)

You will note several fields are marked with a red '*' which means those are required fields. The required fields are your URL and a unique name for your installation. If you are using the default _data_ directory, please note you will need to check the overwrite option. Once you've added your required and optional information click on _next_ to configure your database.

![database configuration](../resources/installation/step-03.png)

The database for TAO can be configured manually or autoconfigured. 

The required fields to manually install are your **database type**, **database host name**, **database user**, **database password**, and the **name for your database**. You can also select whether to overwrite  an existing database with the same name or create the database if it does not already exist. You can also pre-load the database with sample data. Once you've added your required and optional information click on **next** to configure your adminstrator account.

The autoconfigure install will attempt to configure your database utilizing the defaults used by XAMPP.

![admin setup](../resources/installation/step-04.png)

The required fields to configure the user account for the initial administrator access, are **username** and then the **password** and it's confirmation. Once TAO is installed, you can create additional administrative [users](../Management/users.md). Once you've added your required and optional information click on **next** to agree to the relevant licenses.

![licensing](../resources/installation/step-05.png)

There are two licenses you will need to **read and accept** before proceeding. The first is the [GPLv2 License](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html) under which TAO is developed. The second, is the TAO Trademark Community License.

![licensing 2](../resources/installation/step-06.png)

To read and accept the GPLv2 license simply click on the *Read and Accept the GPLv2 License* button which will open a window for you to read the license information. You can then accept the license by clicking on the *I have read and agree to the Terms and Conditions* button. 


![licensing 3](../resources/installation/step-07.png)

Once this is done you will notice a green check mark and that you have reviewed and accepted the terms of the license.

![trademark](../resources/installation/step-08.png)

To read and accept the TAO Trademark Community license simply click on the *Read and Accept the TAO License* button which will open a window for you to read the license information. You can then accept the license by clicking on the *I have read and agree to the Terms and Conditiond* button. 

![licensing 4](../resources/installation/step-09.png)

Once this is done you will notice a green check mark and that you have reviewed and accepted the terms of the license. In addition, the Next button will now be available for you to proceed.

![licensing 5](../resources/installation/step-09.png)

You are not read to finalize your installation by clicking on the green **Install** button.

![finalize](../resources/installation/step-10.png)

As TAO installs, you will see *Deployment in progress* on this screen.

![finalize 2](../resources/installation/step-11.png)

And once completed you will see a window listing the installed extensions.

![install complete](../resources/installation/step-12.png)

Clicking **OK** will take you to the URL of your hostname or IP and you may need to redirect your browser to **http://\<hostname or IP>/tao** to reach your TAO installation.
