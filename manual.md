# Internal ISUCON Competition Day Manual

## Schedule for the Day

  * 12:30 Start of competition
  * 19:00 End of competition

## Portal Site

Please open the link above. Scores measured by the scoring tool will be sent to this portal, where you can view the aggregated results.

## Getting Started

Initially, perform the following actions to check if everything is working properly.

### 2. SSH into the launched EC2 instance as the `ubuntu` user

Example:
```
ssh -i <configured key file> ubuntu@xx.xx.xx.xx
```

After logging in, it is recommended to ensure that you can log in as the `isucon` user.

### 3. Verify the application is working

Access the EC2 instance's public IP address with a browser to verify operation. The following screen should be displayed.

For example, logging in with the username `mary` and password `marymary` should grant you access.

If you cannot access it through a browser, please check with the organizer.

### 4. Perform a load test

After this operation, check the portal to see if your team's score is reflected.

### Directory Structure

The reference implementation application code and the scoring program are located in the `/home/isucon` directory.
```
/home/isucon/
├ env.sh # Environment variables for the application
└ private_isu/
├ webapp/ # Reference implementations in various languages
├ manual.md # This manual
└ public_manual.md # Competition day regulations
```

### How to Switch Implementation Languages

The reference implementation is available in Ruby, PHP, and Go, with Ruby being the default at startup.

You can verify operation from a browser as it is accessible on port 80.

For detailed startup procedures, please refer to `/etc/systemd/system/isu-ruby.service`.

To view logs or errors, use:
```
$ sudo journalctl -f -u isu-ruby
```

Restarting unicorn can be done with:
```
$ sudo systemctl restart isu-ruby
```

#### Switching to PHP

To switch the running implementation to PHP, perform the following:
```
$ sudo systemctl stop isu-ruby
$ sudo systemctl disable isu-ruby
$ sudo rm /etc/nginx/sites-enabled/isucon.conf
$ sudo ln -s /etc/nginx/sites-available/isucon-php.conf /etc/nginx/sites-enabled/
$ sudo systemctl reload nginx
$ sudo systemctl start php8.1-fpm
$ sudo systemctl enable php8.1-fpm
```

Configuration for php-fpm can be found in `/etc/php/8.1/fpm/`.

To view logs or errors, use:
```
$ sudo journalctl -f -u php8.1-fpm
$ sudo tail -f /var/log/nginx/error.log
```

#### Switching to Go

To switch the running implementation to Go, perform the following:
```
$ sudo systemctl stop isu-ruby
$ sudo systemctl disable isu-ruby
$ sudo systemctl start isu-go
$ sudo systemctl enable isu-go
```

For detailed startup procedures, please refer to `/etc/systemd/system/isu-go.service`.

To view logs or errors, use:
```
$ sudo journalctl -f -u isu-go
```

### MySQL

MySQL is running on port 3306. Initially, the following user is configured:

  * Username: `isuconp`, Password: `isuconp`

### memcached

memcached is running on port 11211.

## Detailed Rules

[Internal ISUCON Competition Day Regulations](/public_manual.md)

If there is any inconsistency between the competition day regulations and this manual, the descriptions in this manual take precedence.

### About Scoring

The basic score is calculated as follows:
```
Successful responses count(GET) x 1 + Successful responses count(POST) x 2 + Successful image post responses count x 5 - (Server error(error) responses count x 10 + Request failures(exception) count x 20 + Delayed POST response count x 100)
```

However, if there is a discrepancy between the basic score and the score issued by the scoring tool, the score from the scoring tool will take precedence.

#### Deduction Criteria

Points will be deducted for the following:

  * Failure to access a file that should exist
  * Request failures (e.g., communication errors)
  * Server errors (Status 5xx) and client errors (Status 4xx) returned by the application
  * Other cases detected by the measurement tool's checker

#### Note

  * Redirects count as a successful response if the redirected destination returns the correct response
  * POST failures are subject to significant deductions

### Constraints

Scores will be invalidated if any of the following conditions are met:

  * Response to GET /initialize takes longer than 10 seconds