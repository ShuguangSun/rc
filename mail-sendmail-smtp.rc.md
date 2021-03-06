# Mail

## Send

<http://debian-administration.org/article/171/Send_an_HTML_file_as_email_from_the_command_line>

    echo -e "Subject: Your subject\n\nYour mail body\n" \
         | /usr/sbin/sendmail your.name@your.tld

    echo -e "Your body" | mail --subject="Your subject" your.name@your.tld

    printf %b "From: your.name+1@your.tld\nTo: your.name+2@your.tld\nSubject: Your subject\n\nYour body" | \
           /usr/sbin/sendmail -f "your.name+1@your.tld" "your.name+2@your.tld"

### HTML

Mail with HTML body

```sh
mail --append="Content-type: text/html;" \
     --subject="Your subject" \
     your.name@your.tld < path/to/index.html
```

### SMTP

Send by SMTP

#### Telnet

```sh
telnet localhost smtp
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
220 your.tld ESMTP Exim 4.90_1 Tue, 29 May 2018 07:27:36 +0000
HELO localhost
250 your.tld Hello localhost [127.0.0.1]
mail from: your.name@your.tld
250 OK
rcpt to: your.name@your.tld
250 Accepted
data
354 Enter message, ending with "." on a line by itself
Subject: Your subject

Your mail body
.
250 OK id=1fNZ3C-0001X2-6P
quit
221 your.tld closing connection
Connection closed by foreign host.
```

## Mailserver version

    /usr/sbin/sendmail -bV
    exim -bV

## SMTP

### swaks

`net-mail/swaks` command line smtp testing, including tls and auth
<http://jetmore.org/john/code/swaks/>

    swaks --auth \
          --server your.smtp.domain \
          --auth-user your-login \
          --auth-password your-password \
          --to your.name@your.tld \
          --h-Subject: "Your subject" \
          --body "Your mail body"

## Mailgun

```sh
curl -s --user 'api:YOUR_API_KEY' \
    https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/messages \
    -F from='Excited User <mailgun@YOUR_DOMAIN_NAME>' \
    -F to=YOU@YOUR_DOMAIN_NAME \
    -F to=bar@your.tld \
    -F subject='Hello' \
    -F text='Testing some Mailgun awesomeness!'
```
