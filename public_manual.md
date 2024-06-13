# Internal ISUCON Contest Day Regulations

## Prohibited Actions

The following actions are specifically prohibited:

  * Any act that disrupts other teams, as deemed by the organizers.

## Server Details

Participants will use a single Amazon Web Services (AWS) EC2 instance provided by the organizers.

## Software Details

During the contest, participants may use the software provided or software they implement during the contest period.

As the software to be optimized, the organizers will provide web applications written in Ruby and PHP. However, the organizers do not guarantee that each application's performance will be identical. Participants may use either provided base, or implement their own.

Changes to the following aspects of the given applications are not allowed:

  * The URI of the access destination (port and HTTP request path)
  * The DOM structure of the response (HTML)
  * The content of JavaScript/CSS files
  * The content of media files such as images and videos

There are no restrictions on swapping software on servers, changing settings, modifying application code, etc. The use of external resources beyond the launched instances (such as delegating processing to other instances) is prohibited.

Permitted actions include, for example:

  * Changes to the DB schema and the creation or deletion of indexes
  * Addition of caching mechanisms, job queue mechanisms for deferred writing
  * Reimplementation in other languages

However, participants should take care of the following:

  * Ensure compatibility so that contest progression maintenance commands operate correctly
  * Ensure that server settings and data structures can withstand server reboots at any time
  * Ensure that all application code functions correctly after server reboots
  * Ensure that data written to the application during benchmark runs can be retrieved even after reboots

# Scoring

Scoring will compete on performance values among participants who pass the scoring conditions described below.

Scoring conditions will include passing each of the following checks:

  * During load testing, data posted must be immediately reflected in the associated URI GET response data after returning an HTTP response to the POST.
  * The DOM structure of the response HTML must not have changed.
  * The display and various functions of the page must operate normally when accessing the application from a browser.

Performance metrics will use the following criteria:

  * The execution time of the measurement tool is set to one minute.
    * Detailed thresholds and scoring details will be described in the manual on the day of the event.
  * Scoring will be based on the number of successful HTTP requests within the measurement time.
    * Scoring will vary depending on the type of request.
    * Points will be deducted based on the number of errors.
