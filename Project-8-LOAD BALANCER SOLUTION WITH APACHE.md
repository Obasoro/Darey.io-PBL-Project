# LOAD BALANCER SOLUTION WITH APACHE

When we access the internet using <strong>url</strong> we do not really know how many servers are out there serving our requests, this complexity is hidden from a regular user, but in case of websites that are being visited by millions of users per day (like Google or Reddit) it is impossible to serve all the users from a single Web Server (it is also applicable to databases, but for now we will not focus on distributed DBs).

Each URL contains a domain name part, which is translated (resolved) to IP address of a target server that will serve requests when open a website in the Internet. Translation (resolution) of domain names is perormed by DNS servers, the most commonly used one has a public IP address 8.8.8.8 and belongs to Google. You can try to query it with nslookup command.

The `nslookup` installation on RHEL/CentOs. We must first access it at root user using the `sudo su`

`$ sudo su`

<img width="282" alt="Capture" src="https://user-images.githubusercontent.com/29310552/161868438-054ee781-3fed-47a2-a793-e53b21c9882b.PNG">

Run this command below

`# dnf install bind-utils`

<img width="820" alt="1" src="https://user-images.githubusercontent.com/29310552/161868608-02d577da-45b1-4524-ae35-aceaacfa8443.PNG">

Run the command below to verify installation

`# dig -v`

<img width="365" alt="2" src="https://user-images.githubusercontent.com/29310552/161868756-c22c4ba3-84a1-4f9c-95f8-f0b2a2ca6ad5.PNG">

`nslookup 8.8.8.8`

<img width="364" alt="3" src="https://user-images.githubusercontent.com/29310552/161868852-ad25e9ba-cbe4-41cc-9806-4793f82a7e4f.PNG">



