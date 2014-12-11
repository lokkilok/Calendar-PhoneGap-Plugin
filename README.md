# Ionic Calendar plugin 

for iOS and Android, by [Robert Aarts](http://blog.lokkilok.com)
based upn the Phonegap-Calendar-Plugin by [Eddy Verbruggen](http://www.x-services.nl)

1. [Description](https://github.com/lokkilok/Ionic-Calendar-Plugin#1-description)
2. [Installation](https://github.com/lokkilok/Ionic-Calendar-Plugin2-installation)
3. [Usage](https://github.com/lokkilok/Ionic-Calendar-Plugin#3-usage)
4. [Credits](https://github.com/lokkilok/Ionic-Calendar-Plugin#4-credits)
5. [License](https://github.com/lokkilok/Ionic-Calendar-Plugin#5-license)

## 1. Description

This plugin allows you to add events to the Calendar of the mobile device.

* Works with [Ionic](http://ionicframework.com/) >= 1.0.0-beta13
* Compatible with [Cordova Plugman](https://github.com/apache/cordova-plugman).

### iOS specifics
* Supported methods: `find`, `create`, `modify`, `delete`, ..
* All methods work without showing the native calendar. Your app never looses control.
* Tested on iOS 6 and 7.

### Android specifics
* Supported methods on Android 4: `find`, `create` (silent and interactive), `delete`, ..
* Supported methods on Android 2 and 3: `create` interactive only: the user is presented a prefilled Calendar event. Pressing the hardware back button will give control back to your app.

## 2. Installation

Calendar is compatible with [Cordova Plugman](https://github.com/apache/cordova-plugman) so:
```
$ cordova plugin add https://github.com/lokkilok/Ionic-Calendar-Plugin.git
```
and run this command afterwards:
```
$ ionic build
```

## 3. Usage

Basic operations, you'll want to copy-paste this for testing purposes:

```javascript
  // prep some variables
  var startDate = new Date(2014,10,15,18,30,0,0,0); // beware: month 0 = january, 11 = december
  var endDate = new Date(2014,10,15,19,30,0,0,0);
  var title = "My nice event";
  var location = "Home";
  var notes = "Some notes about this event.";
  var success = function(message) { alert("Success: " + JSON.stringify(message)); };
  var error = function(message) { alert("Error: " + message); };

  // create a calendar (iOS only for now)
  window.plugins.calendar.createCalendar(calendarName,success,error);
  // if you want to create a calendar with a specific color, pass in a JS object like this:
  var createCalOptions = window.plugins.calendar.getCreateCalendarOptions();
  createCalOptions.calendarName = "My Cal Name";
  createCalOptions.calendarColor = "#FF0000"; // an optional hex color (with the # char), default is null, so the OS picks a color
  window.plugins.calendar.createCalendar(createCalOptions,success,error);

  // delete a calendar (iOS only for now)
  window.plugins.calendar.deleteCalendar(calendarName,success,error);

  // create an event silently (on Android < 4 an interactive dialog is shown)
  window.plugins.calendar.createEvent(title,location,notes,startDate,endDate,success,error);
  
  // create an event silently (on Android < 4 an interactive dialog is shown which doesn't use this options) with options:
  var calOptions = window.plugins.calendar.getCalendarOptions(); // grab the defaults
  calOptions.firstReminderMinutes = 120; // default is 60, pass in null for no reminder (alarm)
  calOptions.secondReminderMinutes = 5;

  // Added these options in version 4.2.4:
  calOptions.recurrence = "monthly"; // supported are: daily, weekly, monthly, yearly
  calOptions.recurrenceEndDate = new Date(2015,6,1,0,0,0,0,0); // leave null to add events into infinity and beyond
  calOptions.calendarName = "MyCreatedCalendar"; // iOS only
  window.plugins.calendar.createEventWithOptions(title,location,notes,startDate,endDate,calOptions,success,error);

  // create an event interactively (only supported on Android)
  window.plugins.calendar.createEventInteractively(title,location,notes,startDate,endDate,success,error);

  // create an event in a named calendar (iOS only for now)
  window.plugins.calendar.createEventInNamedCalendar(title,location,notes,startDate,endDate,calendarName,success,error);

  // find events (on iOS this includes a list of attendees (if any))
  window.plugins.calendar.findEvent(title,location,notes,startDate,endDate,success,error);

  // list all events in a date range (only supported on Android for now)
  window.plugins.calendar.listEventsInRange(startDate,endDate,success,error);

  // list all calendar names - returns this JS Object to the success callback: [{"id":"1", "name":"first"}, ..]
  window.plugins.calendar.listCalendars(success,error);

  // find all events in a named calendar (iOS only for now, this includes a list of attendees (if any))
  window.plugins.calendar.findAllEventsInNamedCalendar(calendarName,success,error);

  // change an event (iOS only for now)
  var newTitle = "New title!";
  window.plugins.calendar.modifyEvent(title,location,notes,startDate,endDate,newTitle,location,notes,startDate,endDate,success,error);

  // delete an event (you can pass nulls for irrelevant parameters, note that on Android `notes` is ignored). The dates are mandatory and represent a date range to delete events in.
  // note that on iOS there is a bug where the timespan must not be larger than 4 years, see issue 102 for details.. call this method multiple times if need be
  window.plugins.calendar.deleteEvent(newTitle,location,notes,startDate,endDate,success,error);
  
  // open the calendar app (added in 4.2.8):
  // - open it at 'today'
  window.plugins.calendar.openCalendar();
  // - open at a specific date, here today + 3 days
  var d = new Date(new Date().getTime() + 3*24*60*60*1000);
  window.plugins.calendar.openCalendar(d, success, error); // callbacks are optional
```

Creating an all day event:

```javascript
  // set the startdate to midnight and set the enddate to midnight the next day
  var startDate = new Date(2014,2,15,0,0,0,0,0);
  var endDate = new Date(2014,2,16,0,0,0,0,0);
```

Creating an event for 3 full days

```javascript
  // set the startdate to midnight and set the enddate to midnight 3 days later
  var startDate = new Date(2014,2,24,0,0,0,0,0);
  var endDate = new Date(2014,2,27,0,0,0,0,0);
```


## 4. CREDITS ##

This plugin was is based by the PhoneGap-Calendar-Plugmin by [Eddy Verbruggen](http://www.x-services.nl).
The main difference is that this version makes use of event ids, so that changes to events can be propagated to
entries in the calendar.
Also the JS in this package is packages as an angular module which is the more natural way to add it to an Ionic project.

## 5. License

[The MIT License (MIT)](http://www.opensource.org/licenses/mit-license.html)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
