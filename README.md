# DateTimeCheatsheet

[Published on my blog here as well](https://www.reesskennedy.com/computer-date-time-cheatsheet/)

Table of Contents
[About](#about)
[String to Timestamp](#string-to-timestamp)
 [PHP](#php)
 [Python](#python)
 [Bash](#bash)
 [Javascript](#javascript)
[On Timezones](#on-timezones)
[Time Formats](#time-formats)
 [24-Hour Time Format (Military Time):](#24-hour-time-format-military-time)
 [12-Hour Time Format (AM/PM):](#12-hour-time-format-ampm)
 [ISO 8601 Date and Time Format:](#iso-8601-date-and-time-format)
 [Unix Timestamp (Epoch Time):](#unix-timestamp-epoch-time)
 [Human-Readable Date and Time Formats:](#human-readable-date-and-time-formats)
 [Custom Date and Time Formats:](#custom-date-and-time-formats)
 [Relative Time Formats:](#relative-time-formats)
 [Localized Time Formats:](#localized-time-formats)

## About
Below are some code examples for common date and time conversion in different languages and an index of common ways time is stored and understood by a computer. 

Reason for writing this out: I have wasted time looking up the same DateTime info on a reoccurring basis and decided I might end this by creating this cheatsheet for personal reference, and to share. Manipulating dates is very common when coding a tool or app but given the importance of maintaining the proper file creation date for photographs and other digital assets, I'd argue that understanding a bit about computer time is important for anyone who uses a computer and wants to keep their assets well organized. 

## String to Timestamp
Examples below in PHP, Python, Bash and Javascript. 
### PHP

```php
$timestamp = strtotime("2023-09-24 14:30:00");
```

`strtotime()` function is very flexible and can take a wide variety of date and time string formats and convert them to a Unix timestamp. 

On the other hand, `time()` is a simple PHP function that returns the current Unix timestamp, represending the current date and time. Example:

```php
$currentTimestamp = time();
```

### Python
A similar conversion using Python. 
```python
from datetime import datetime

date_string = "2023-09-24 14:30:00"
date_format = "%Y-%m-%d %H:%M:%S"

# Parse the date string into a datetime object
date_object = datetime.strptime(date_string, date_format)

# Convert the datetime object to a Unix timestamp
timestamp = date_object.timestamp()

print(timestamp)

```

### Bash
And plain-old bash is quite terse!
```bash
date -d "2023-09-24 14:30:00" "+%s"
```

Explanation: 
- `-d` specifies the date and time string to be parsed.
- `"2023-09-24 14:30:00"` is the date and time string you want to convert.
- `"+%s"` is the format specifier that tells `date` to output the result as a Unix timestamp.

### Javascript

```js
// Input date string
var dateString = "2023-09-24 14:30:00";

// Parse the date string into a Date object
var dateObject = new Date(dateString);

// Get the Unix timestamp (milliseconds since January 1, 1970)
var timestamp = dateObject.getTime() / 1000; // Convert milliseconds to seconds

console.log(timestamp);
```


## Troubleshooting Conversion

### Locale Settings Modification
The string to time function is flexible to understand different date entries like that "9/18/2023" and "18-09-23" mean the same day BUT there are times like when you have "09-08-23" when the function won't know whether things string value means September 9th or August 8th and will, by default, choose a "locale settings"--how you have PHP configured. 

To ensure consistent date parsing regardless of locale settings, you can use the `DateTime` class and specify the desired format explicitly. Here's how you can modify the code to handle "MM-DD-YYYY" format:

```php
// Set the desired locale format (for example, to use "DD-MM-YYYY" format)
setlocale(LC_TIME, 'en_US.utf8'); // Change 'en_US.utf8' to your desired locale

$dateString = '09-18-2023 13:34:46';

$timestamp = strtotime($dateString);
```

## On Timezones

**Easy to receive as a timestamp**
This makes it easy because the timestamp is in seconds from a fixed date ("January 1, 1970, at 00:00:00 UTC") this value will be the same for everyone. 

**Easy if just converting to timestamp**
Even though "2023-09-24 14:30:00" will be a different time in New York than Chicago because they are different timezones the beauty of the timestamp is that the timestamp value for will not be different since it's just counting out from a fixed point. The time we need to worry bout timezones is when we have to consider the timezones of the incoming date data we're receiving. 

**Strings may require timezone conversion**
If you're just working with your own data and you're always working on the same timezone then this isn't an issue but if you travel around a lot and are pulling data from your analytics logs or elsewhere you start to have to consider the time zone and define this before you convert this value to a timestamp. 

Something like this in PHP
```php
// Input timestamp string
$inputTimestamp = '2023-09-24 15:30:00';

// Convert the timestamp string to a Unix timestamp
$unixTimestamp = strtotime($inputTimestamp);

// $unixTimestamp now contains the Unix timestamp
echo $unixTimestamp; // Output: 1693053000

```

## Time Formats
### 24-Hour Time Format (Military Time):
Example: 14:30 (2:30 PM in 12-hour format) This format represents time in a 24-hour cycle, with hours ranging from 00 to 23.

### 12-Hour Time Format (AM/PM):
Example: 2:30 PM (or 14:30 in 24-hour format) This format represents time in a 12-hour cycle, with hours ranging from 1 to 12, accompanied by "AM" (Ante Meridiem) for times before noon and "PM" (Post Meridiem) for times after noon.

### ISO 8601 Date and Time Format:
Example: 2023-09-24T14:30:00Z This format is an international standard and includes the date in YYYY-MM-DD format followed by the time in HH:MM:SS format, with an optional time zone indicator. The "T" separates the date and time, and "Z" indicates UTC (Coordinated Universal Time).

> Note: Some popular API's, like the Raindrop API, use ISO 8601 format.

### Unix Timestamp (Epoch Time):
Unix timestamp = the number of seconds since January 1, 1970

Example: 1632471000 Unix timestamps represent time as the number of seconds that have elapsed since January 1, 1970, at 00:00:00 UTC (Coordinated Universal Time). It is often used in computing for timestamping and calculating time intervals.

### Human-Readable Date and Time Formats:
These formats include variations of day, month, year, and time representations like "Monday, September 24, 2023, 2:30 PM."

### Custom Date and Time Formats:
Depending on the application and user preferences, custom date and time formats can be created, specifying how time should be displayed. For example, "dd/MM/yyyy HH:mm:ss" or "MM/dd/yyyy h:mm a" for custom date and time formatting.

### Relative Time Formats:
These formats show time relative to the current moment, like "just now," "2 minutes ago," or "yesterday." They are commonly used in messaging apps and social media platforms.

### Localized Time Formats:
Time formats can vary by region and language. Computers often adapt to the user's locale and display time in a format consistent with local conventions.

