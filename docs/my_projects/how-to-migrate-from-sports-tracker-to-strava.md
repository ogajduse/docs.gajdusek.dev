# How to migrate activities from Sports Tracker to Strava

One day I came across my old and unused Sports Tracker account in which I still
have some activities uploaded. That day I decided to migrate them to Strava as
it is the tool that I currently use for tracking my activities. In the next few
steps, I will try to guide you through the migration process.

## Create Strava application

Go to <https://www.strava.com/settings/api> and create a new API application.

Choose a cool name for your application, leave the default value in the category
field, put some website in the _Website_ field, even a fake one will do. Into
the _Authorization Callback Domain_ put `developers.strava.com` and create your
API application.

When you will go to the
[My API Application](https://www.strava.com/settings/api) page, you should see
that a _Client ID_ and _Client Secret_ were generated for you. Also, see the
_Rate Limits_ which restrict you from making more requests than it specifies.

## Generate API token

Next, you need to obtain the authorization token. I studied it a bit and figured
out that Strava API provides some APIs that should provide me with the token.
However, I come up with a faster solution. Open
[Strava Swagger API doc](https://developers.strava.com/playground/) and click
the _Authorize_ button on the right. Put there _client\_id_ and _client_secret_
from [My API Application](https://www.strava.com/settings/api) page and in the
_Scope_ section select only `activity:write` and hit _Authorize_ button. You
will get redirected to the page where you just confirm that your API application
does have the right to upload activities. On this page hit _Authorize_ button.
Now you should see available authorization. From the Swagger API doc, choose the
`GET /athlete Get Authenticated Athlete` endpoint, hit _Try it out_ button and
hit _Execute_. In the _Responses_ section you should see a curl command
generated with an HTTP header option filled with a token. Something like
`-H "authorization: Bearer 268xxxxxc67d820xxxxxxxxxx0b2xxxxxxxxxxe9"` Copy the
token to the clipboard.

## Download helper scripts

Next, download the following two files from the gist.
<https://gist.github.com/ogajduse/6d13b367f92d2af078704b6777d50c09>

## Download activities from Sports Tracker

Navigate to <https://www.sports-tracker.com/diary/workout-list> and keep hitting
_Show more_ button on the bottom of the page until you load all your activities
on the single page. Open browser console by pressing `Ctrl + Shift + I`. Copy
the contents of the `sports-tracker-download.js` file from the gist and paste it
to the console and hit `Enter`. Right-click to the console output and save the
log to disk. Open the file and remove the script part on the top that you pasted
to the console. Now remove all the line prefixes if there are any. I used the
following sed command to remove them.

```bash
sed -i 's/^VM[0-9]*\:[0-9]*\s//' www.sports-tracker.com-1645568542000.log
```

Now you should have a nice bash script that you can run and create an offline
backup of all your Sports Tracker activities. Move the script to the same
directory where you have downloaded the files from the gist and run it.

```bash
bash www.sports-tracker.com-1645568542000.log
```

It should create a directory structure in which the GPX files and images are
stored. Each directory represents single Sports Tracker activity. You can ZIP
this folder and back it up as an offline backup.

## Upload activities to Strava

Once you have dowloaded the activities, we have to upload them to Strava. The
gist that I created provides an `activities-upload.sh` helper script for that.
It is written in Bash and it requires standard GNU tools,
[jq](https://stedolan.github.io/jq/download/), curl and Perl installed on the
system. You need to do add a Sports Tracker username to the script as well as
the Strava API token as follows.

```text
TOKEN=268xxxxxc67d820xxxxxxxxxx0b2xxxxxxxxxxe9 # Strava token
ST_USERNAME=johndoe # Sports Tracker username
```

Now we are all set for the upload. Execute the upload script.

```bash
bash activities-upload.sh
```

You should see that it goes through all the activities that ar found in the
current directory and it uploads them to Strava. When the script finishes, you
should see a summary of what was done. Each time the script runs, there is a new
_results.json_ file created with the timestamp when it was executed. For more
information see the JSON file contents.

If there are any photos to upload to your activities, the script will notify you
in the summary section.

```text
There are photos that might be uploaded for the following activities:
https://www.strava.com/activities/<your-activity-id>
Upload the following photos to this activity at https://www.strava.com/activities/<your-activity-id>/edit
<sports-tracker-activity-id>/<sports-tracker-activity-id>-531348c3e4b0d9b3df012aa3-20.63160584-47.6522775-1393772737188.jpg
```

## Conclusion

That's it, we have successfully uploaded activities from Sports Tracker to
Strava. You can check the uploaded activities at
<https://www.strava.com/athlete/training>. If you would like to explore the
Strava API more, check out <https://developers.strava.com/docs/reference/>.

## Links that I found along the way

### Tooling

[stravacli](https://github.com/dlenski/stravacli) - Command-line client for Strava.

[strava-uploader](https://github.com/barrald/strava-uploader) - personal
project on GitHub that uploads activities from RunKeeper.

[stravaweblib](https://github.com/pR0Ps/stravaweblib) - Provides all the functionality
of the stravalib package and extends it using web scraping.

### Other links

stackoverflow discussion about the access token generation <https://stackoverflow.com/questions/52880434/>.
Strava developer document describing API authentication. <https://developers.strava.com/docs/authentication/>.
