% Teaching Someone To Phish  
% https://github.com/STL2600/

# Disclaimer

- Educational purposes only
- Likely violating some ToS
- With permission only. As legitimate test.

::: notes

- Disclaimers
    
- Educational purposes only
- Likely violating some ToS
    - With permission only. As legitimate test.  
:::

# Recon

- Who Is your Target?
- Cloud and Email Services
- Check Social Media

::: notes  
Step 1: As with any engagement.

- Who are your targets?
- What does your target use for email and cloud services?
    - This will be important later when building our own infrastructure
- Check social medias for current events
    - Any celebrations or announcments?
    - Employees complaining about changes?
    - General calendar holidays

:::

# Planning

- Notifying
- Budget
- What are we testing for?

;;; notes

```
- Notify relevant parties
  - Someone at the company you are testing should obviously know.
  - Probalby their email admin in case your tests do get blocked
    - Because successful phishing emails are actually kind of hard
	- Anyone that allows you to test is usually pretty good about blocking things quickly
  - The VPS and mailing service?
    - Technically you probably should
	- Most likely you are violating ToS and they'll just shut you down.
- Budget
  - Cost for a domain name
  - VPS services
  - Mailing service
  - Make use of free tiers when you can?
- What are you testing
    - Credentials?
    - For which site?
    - Target behavior (Do they open it)?
```

;;;

## Planning (cont)

- Your list of targets
- Choose our services
- Select a domain name
- The story

;;; notes

- Planning
    
    - List of targets
        - Your list of emails of course
    - What services are you using.
        - Good idea to match your VPS with the targets cloud provider
        - i.e. Azure for Azure, AWS for AWS
    - Pick a convincing looking domain
    - The story
        - This is why we checked social media.
        - Did they have a baseball opening day party recently? Here's a link to the photos
        - Were there policy changes being announced? Here's a link to an FAQ.
        - Or does the CEO just really need some boner pills.  
            ;;;

# Infrastructure

- Hosting Provider
- GoPhish
- Evilginx
- The Domain
- Mailing Service

;;; notes  
;;;

## Hosting Provider

- Who is your target using for email?

;;; notes

- Comes down to your target and your story
- If your target uses 0365, and your are offering a fake login page from AWS, that could look suspicious to an IDS

;;;

## GoPhish

- Campaign Managment Tool
- Configure Email Template
- Launch Emails
- Track Results

;;; notes

- Campaign Managment Tool
- Configure Email Template
- Launch Emails
- Track Results

;;;

## Evilginx

- MitM to capture sessions
- Hosts fake login pages

;;; notes

- MitM framework to capture logins and sessions tokens
- Allows us to bypas 2FA
- Hosts fake login pages
- Originally a fork of nginx, hense the name
- Has since been completely re-written in go as a standalone app

;;;

## Domain

- Typos
- Expired Domains
- Site Categorization

;;; notes

Your domain choice is going to reflect the story you are telling

;;;

### URLCrazy

`urlcrazy -o ./urls.csv -f csv archreactor.org`  

### URLCrazy
![0d275807f30695ba3c15b208866e2b83.png](:static/c425f64a85314a37b0aa2432dbe5dd8c.png)  
;;; notes  
;;;

### Expired Domains

![b515f8d490246d907a77d762c67b1b19.png](:static/9359ca3dc4564f16a47fccb0160565bb.png)

![8da537d26f14a4adc1692559c617d56d.png](:static/ea964832ce5b4564bf441877be2e7b7b.png)  
;;; notes  
;;;

### Site Categorization

&nbsp;
|     |     |
| --- | --- |
| https://www.fortiguard.com/iprep | https://global.sitesafety.trendmicro.com/ |
| https://sitereview.bluecoat.com/#/ | https://www.brightcloud.com/tools/url-ip-lookup.php |
| https://talosintelligence.com/ | https://archive.lightspeedsystems.com/ |
| https://urlcat.checkpoint.com/urlcat/main.htm | https://multirbl.valli.org/ |
| https://urlfiltering.paloaltonetworks.com/ | https://mxtoolbox.com/blacklists.aspx |

;;; notes  
;;;

### Site Categorization
![bc34b9cccd36b77c9483bf7cfd12c0f8.png](:static/4bd5f1d9bec74e21a6f6d31544a550f8.png)

### Site Categorization
![2467bff69ec7a694f6e8732230c15973.png](:static/fe1097fa342241cfb178ca3a8eb74f90.pmg)

## Mailing Service

- Mailchimp
- Mailgun
- Sendgrid
- Postfix
- Outlook.com or Gmail.com
- 365 Tenant

### Mailing Services
![90a618c60202904ec869bb7e57148cb5.png](:static/8728a3a0b7d7448f8d02f075a5f8cd83.png)
;;; notes  
;;;

# Anger the Demo Gods

;;; notes
- DMARC

- RID

Looking closely, we are interesting for the following POST structure request, and more specific for the parameter that holds the current login id.

https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=

In order to reflect the actual Microsoft’s connection configuration, we can modify the RecipientParameter parameter Under /gophish/models/campaign.go file path.

const RecipientParameter = "cliend_id"

- Preventing
    
    - External Email flags\
;;;
