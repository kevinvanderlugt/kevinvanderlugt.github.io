---
layout: post
title:  "Building a custom iOS mailing list subscription with MailChimp"
date:   2014-05-29 10:00:00
categories: blog
---

One of the biggest regrets I have throughout releasing multiple applications is not building a mailing list of loyal fans earlier.  Learn from my mistake and use this one feature to significantly increase your customer reputation and branding.

The benefits of having a mailing list of users are probably obvious but I wish I added this simple feature into our initial releases.

Some of the benefits of building your list:  
- The ability to send emails when you release updates or new apps  
- Cross promotion between apps for your brand  
- Easier to interact with your customers to get app feedback  

Enough about the benefits already, lets get down to building it.  I chose to use [MailChimp](http://www.mailchimp.com) mostly because I had experience with it.  They have a great free option and it's not too expensive once you get over 10,000 users but by then this list should be paying for itself.

MailChimp has great coverage of their API documentation and even created a [wrapper](https://github.com/mailchimp/ChimpKit3) around their API for objective-c.  We will use NSUrlConnection instead to keep the dependencies small and because we only need to make one API call to subscribe our users.

A few things we will need first before we can jump into the code.  

1.  Create and save an API key from MailChimp.  Currently this can be done under Profile -> Extras -> API Keys but this could change.
2.  You need to also have a list already generated that will be used to keep all your users.  Under your list settings you will need to get the List ID to use for our API call.

As far as the design goes for the list subscribe goes in your app, we will leave that up to you.  We will create a new UIViewController that will pop onto our navigation controller.

On this new view controller you should add three UITextInput fields and link them to the interface extension in your new view controller

{% highlight objective-c linenos %}
@interface EmailSubscribeViewController ()

@property (weak, nonatomic) IBOutlet UITextField *emailField;
@property (weak, nonatomic) IBOutlet UITextField *firstNameField;
@property (weak, nonatomic) IBOutlet UITextField *lastNameField;

@end
{% endhighlight %}

Next, we need to create a method for when the user submits their information.  For the sake of simplicity, I have called this submitPressed but you could use whatever you want.  The method will create the post body and send a HTTP POST request to the MailChimp API.

{% highlight objective-c linenos %}
- (IBAction)submitPressed:(id)sender {
    // Get all the user inputs, no real validation is done here but you could
    NSString *email = self.emailField.text;
    NSString *firstName = self.firstNameField.text;
    NSString *lastName = self.lastNameField.text;
    
    // The MailChimp API that we are using was version 2.0, this may change
    // API_ZONE should be replaced with your API zone
    // This can be found at the end of your API key, mine was us8
    NSString *urlString = @"https://API_ZONE.API.mailchimp.com/2.0/lists/subscribe";
    NSMutableURLRequest *urlRequest =
        [NSMutableURLRequest requestWithURL:[NSURL URLWithString:urlString]
                                cachePolicy:NSURLRequestUseProtocolCachePolicy
                            timeoutInterval:60.0];
    
    [urlRequest setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
    [urlRequest setHTTPMethod:@"POST"];
    
    // Create the body using the native json generator
    NSMutableDictionary *body = [[NSMutableDictionary alloc] init];

    // Replace API_KEY with your API key found earlier
    [body setObject:@"API_KEY" forKey:@"APIkey"];

    // Replace LIST_ID with your list ID
    [body setObject:@"LIST_ID" forKey:@"id"];

    NSMutableDictionary *emailDictionary = [[NSMutableDictionary alloc] init];
    [emailDictionary setObject:email forKey:@"email"];
    
    [body setObject:emailDictionary forKey:@"email"];
    
    // Merge variables are values passed into your list including user's name
    NSMutableDictionary *mergeVariables = [[NSMutableDictionary alloc] init];
    if(firstName) {
        [mergeVariables setObject:firstName forKey:@"FNAME"];
    }
    if(lastName) {
        [mergeVariables setObject:lastName forKey:@"LNAME"];
    }
    
    [body setObject:mergeVariables forKey:@"merge_vars"];
    
    // Optionally you can disable email verification
    [body setObject:@NO forKey:@"double_optin"];
    
    NSData *requestBody = [NSJSONSerialization dataWithJSONObject:body options:0 error:nil];
    
    [urlRequest setHTTPBody:requestBody];
    
    [NSURLConnection sendAsynchronousRequest:urlRequest
                                       queue:[NSOperationQueue mainQueue]
                           completionHandler:^(NSURLResponse *response, NSData *responseData, NSError *error){
                               if (error != nil) {
                                   NSLog(@"Error subscribing to the list: %@", [error localizedDescription]);
                               } else {
                                  // Do something useful here to thank your users
                                  NSLog(@"Success, thanks for signing up!");
                               }
                           }];
}
{% endhighlight %}

Thats all you need for a simple email signup list in your iOS application.  This feature can be a huge business value to any application for a relatively simple implementation.  I highly recommend anyone use some sort of mail subscription service at the beginning of their apps.

There is still lots of customizations that you can do based on the MailChimp documentation.  You should definitely poke through their documentation, there are other merge variables that can be used to identify the application or the particular screen that users signed up for.  This additional customization would depend on your business goals so I decided to leave them out.

The full code for this view controller can be found in [this gist](https://gist.github.com/kevinvanderlugt/7532b4cdb46f33068df2) for easier copying.

Cheers!  
-Kevin

---

Since this is my first blog post, I want to know what you thought when reading.  How can I improve the quality of the post?  Any thing you wish you saw earlier in your project.  [Email me](mailto:kevin.vanderlugt@gmail.com) your comments and lets chat!