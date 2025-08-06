# Multipagewebsite-AWS-SES-Lambda
Set up an email contact to send messages to your Gmail with AWS SES (Simple Email Service) and AWS Lambda 

# Overview of the complete setup

1. Frontend Contact Form (HTML + JS)

2. API Gateway (trigger)

3. Lambda Function (backend to process and send email)

4. AWS SES (to send the email to your Gmail)

Lets Begin 

1. Step We confirmed our Domain to SES by verifying our identities

![alt text](<Bilder/Screenshot (302).png>)

2. Once verified identity to verify ownership of domain We can now use contact@denisriungu.de to send messages via SES (You cannot use gmail because you don't own gmail.com)

3. I left the default settings Easy DKIM anabled, DKIM Signatures also enabled, Publish DNS records to Route 53: Leaving this checked (as we are going to be using Route 53 for DNS).

![alt text](<Bilder/Screenshot (303).png>)

4. Went to Route 53 → Hosted Zones (denisriungu.de) and checked out if the DKIM CNAME records already have been added automatically.

![alt text](<Bilder/Screenshot (305).png>)

5. In 30 minutes, SES detected the records and verified the domain.

![alt text](<Bilder/Screenshot (306).png>)

6. Next we wrote my HTML Contact Form and deployed it on my website contact page on out AWS S3 Bucket.

![alt text](<Bilder/Screenshot (307).png>) html Contact form

![alt text](<Bilder/Screenshot (308).png>) Fetch API Javascript.

7. I then created a Lambda function to handle the contact form on the resume website.

![alt text](<Bilder/Screenshot (309).png>)

8. then created a new role with basic Lambda Permissions. 

![alt text](<Bilder/Screenshot (310).png>)

9. Then i added SES permissions and attached Policies to the Lamda Roles

![alt text](<Bilder/Screenshot (311).png>)

attached policy

![alt text](<Bilder/Screenshot (312).png>)

10. Searched for AmazonSESFullAccess and Attached policy

![alt text](<Bilder/Screenshot (313).png>)


Prerequisites

Your Lambda function (e.g., sendContactEmail) is working and has SES permissions.

You are ready to connect this function to an API Gateway endpoint.

11. Create a HTTP API and intergrate it with the lambda function 
 Why not REST API. Why HTTP API?
 HTTP API for simplicity and cost and in the case of sending emails from my resume website to my gmail account. it suits better compared to REST API which is more expensive and used when you need advanced API features.

Integration target: Lambda function

![alt text](<Bilder/Screenshot (315).png>)

Lambda function: sendContactEmail

![alt text](<Bilder/Screenshot (316).png>)

12. # Enable CORS (Cross-Origin-Resource-Sharing)'#

![alt text](<Bilder/Screenshot (317).png>)

13. Invoke the URL to Deploy the API 

![alt text](<Bilder/Screenshot (318).png>)

14. Copy this URL — you’ll use it in your HTML contact form!

![alt text](<Bilder/Screenshot (319).png>)

15. Test to see if message can be sent? 
Message is sent successfull but not received on my gmail since our AWS account is still on sandbox mode. 

 ![alt text](<Bilder/Screenshot (209).png>)

 16. and we need to request SES production access.

 ![alt text](<Bilder/Screenshot (321).png>)

 17. The Request Got Rejected.

 ![alt text](<Bilder/Screenshot (320).png>)

 18. Tried again. Never Gave Up and an re-applied and clarifed everything even further.

 ![alt text](<Bilder/Screenshot (322).png>)

 19. Success. Got Approved for SES production access.

 ![alt text](<Bilder/Screenshot (323).png>)

 20. Ran a Test. Send Message from contact form on denisriungu.de

 ![alt text](<Bilder/Screenshot (209).png>)

 21. and success. I can receive messages from denisriungu.de contact form on the contact page to my gmail account. 

 ![alt text](<Bilder/Screenshot (206).png>)

 


Very important lesson i learned when working with fullstack development with APIs is that CORS (Cross-Origin Resource Sharing) is crucial in this setup because your contact form (frontend) is hosted on a different domain (e.g., your website on S3) than your backend API (API Gateway + Lambda). By default, browsers block requests from one origin (domain) to another for security reasons.

Enabling CORS on your API Gateway allows your frontend JavaScript to send requests to your Lambda function endpoint. Without CORS, the browser would block these requests, and your contact form would not be able to send emails via the API. CORS ensures secure and controlled cross-domain communication, which is essential for your contact form to work.


22. # Created SNS Topics for Bounce & Complaint Notifications #

. In order to track if emails bounce(eg if user mispelled their email address), Send alerts to my email or to store in Cloudwatch Logs.

![alt text](<Bilder/Screenshot (324).png>)

. Set up.

![alt text](<Bilder/Screenshot (325).png>)

 . With Event Destination To Cloudwatch to send alerts to store in Cloudwatch Logs.

![alt text](<Bilder/Screenshot (329).png>)

 . Viewing Metrics in CloudWatch 

![alt text](<Bilder/Screenshot (330).png>)

 . Test To see if the SNS to cloudwatch works and sends metrics to Cloudwatch by Successfully sending Test message to myself.

![alt text](<Bilder/Screenshot (332).png>)

 . Successfully received 

![alt text](<Bilder/Screenshot (336).png>)

 . and Delivery Metrics in CloudWatch reflect exacly the same thing. 

![alt text](<Bilder/Screenshot (334).png>)

. Check Bounce and Complaint Metrics and that nobody flagged my email.

![alt text](<Bilder/Screenshot (337).png>)

and all that is also possible because we configured 

 23. AWS Lambda to Use the SES send_email call Configuration Set to Cloudwatch.

 ![alt text](<Bilder/Screenshot (338).png>)

 24. 








