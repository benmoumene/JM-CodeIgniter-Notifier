JM-CodeIgniter-Notifier
=======================

This code is designed to make it very easy to have a centralized notifications handler for your CodeIgniter application. 

The idea is that you can have html email templates as individual html files making it easy for front end developers to design and build, while providing a simple means for us back end guys to send rich html emails (with attachments).

The included _CI_Notifications_ class is essentially an extendible wrapper for the CodeIgniter email helper.

## Installation
### Setting up the classes
Simply copy the _CI_Notifications_ class file and the _Example_Notifications_ class file to your _application/libraries_ folder (rename the Example_Notifications file and class to whatever you want). From there you'll want to add it to the autoloader ( _application/config/autoload.php_ ) in the libraries array.

### Create an email template directory
Each notification email is an html file on its own. We need a directory to store them in, so I normally create _./public/email-templates/_ in which I store the template files and their assets (normally all stored in a sub directory).

### Configuring your notifications class
There are a few variables in the constructor of your notifications class that will now need setting up.

    # Email Configuration
    $this->_emailFromAddress    = 'no_reply@mysite.com';
    $this->_emailFromName       = 'My Site Admin';
    $this->_emailReplyToAddress = 'no_reply@mysite.com';
    $this->_emailTemplateDir    = 'public/email-templates/';
    
## Usage
Now that your classes are configured, you can set up a method to send a notification. The below examples is taken from the Example_Notifications class.
    
    /**
     * This is an example method that sends a welcome email out.
     * 
     * @param <string> $name
     * @param <string> $password
     * @param <string> $emailAddress 
     * return <boolean>
     */
    public function sendWelcomeEmail($name, $password, $emailAddress) {
        
        # Read in template contents
        $template = file_get_contents($this->_emailTemplateDir . 'welcome-email.html');
        
        # Replace template variables
        $template = str_replace('{name}', $name, $template);
        $template = str_replace('{email}', $emailAddress, $template);
        $template = str_replace('{password}', $password, $template);
        $template = str_replace('{site_url}', $this->_siteUrl, $template);
                
        # Send the email
        return parent::_sendEmailNotification(
                    $emailAddress, 
                    "Welcome to the the internets.", 
                    $template, 
                    'Plaintext goes here.', # Optional
                    'Attachment.txt' # Optional
                ); 
    }
    
Once you have set up your notifications as above, you can send out your notification email from your code with ease.

    # Create an instance of the notifications handler and send the welcome email
    $notificationHandler = new Example_Notifications();
    $notificationHandler->sendWelcomeEmail("Billybob", "asd123", "billybob@gmail.com");