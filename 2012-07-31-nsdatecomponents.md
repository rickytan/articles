---
layout: post
title: NSDateComponents
ref: "https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/NSDateComponents_Class/Reference/Reference.html"
framework: Foundation
rating: 6.9
description: "NSDateComponents serves an important role in Foundation's date and time APIs. By itself, it's nothing impressive—just a container for information about a date (its month, year, day of month, week of year, or whether that month is a leap month). However, combined with NSCalendar, NSDateComponents becomes a remarkably convenient interchange format for calendar calculations."
---

`NSDateComponents` serves an important role in Foundation's date and time APIs. By itself, it's nothing impressive—just a container for information about a date (its month, year, day of month, week of year, or whether that month is a leap month). However, combined with `NSCalendar`, `NSDateComponents` becomes a remarkably convenient interchange format for calendar calculations.

Whereas dates represent a particular moment in time, date components depend on which calendar system is being used to represent them. Very often, this will differ wildly from what many of us may be used to with the [Gregorian Calendar](http://en.wikipedia.org/wiki/Gregorian_calendar). For example, the [Islamic Calendar](http://en.wikipedia.org/wiki/Islamic_calendar) has 354 or 355 days in a year, whereas the [Buddhist calendar](http://en.wikipedia.org/wiki/Buddhist_calendar) may have 354, 355, 384, or 385 days, depending on the year.

## Extracting Components From Dates

`NSDateComponents` can be initialized and manipulated manually, but most often, they're extracted from a specified date, using `NSCalendar -components:fromDate:`:

~~~{objective-c}
NSCalendar *calendar = [NSCalendar currentCalendar];
NSDate *date = [NSDate date];
[calendar components:(NSDayCalendarUnit | NSMonthCalendarUnit) fromDate:date];
~~~

The `components` parameter is a [bitmask](http://en.wikipedia.org/wiki/Bitmask) of the date component values to retrieve, with many to choose from:

- `NSEraCalendarUnit`
- `NSYearCalendarUnit`
- `NSMonthCalendarUnit`
- `NSDayCalendarUnit`
- `NSHourCalendarUnit`
- `NSMinuteCalendarUnit`
- `NSSecondCalendarUnit`
- `NSWeekCalendarUnit`
- `NSWeekdayCalendarUnit`
- `NSWeekdayOrdinalCalendarUnit`
- `NSQuarterCalendarUnit`
- `NSWeekOfMonthCalendarUnit`
- `NSWeekOfYearCalendarUnit`
- `NSYearForWeekOfYearCalendarUnit`
- `NSCalendarCalendarUnit`
- `NSTimeZoneCalendarUnit`

> Since it would be expensive to compute all of the possible values, specify only the components that will be used in subsequent calculations (joining with `|`, the bitwise `OR` operator).

## Relative Date Calculations

`NSDateComponents` objects can be used to do relative date calculations. To determining the date yesterday, next week, or 5 hours and 30 minutes from now, use `NSCalendar -dateByAddingComponents:toDate:options:`:

~~~{objective-c}
NSCalendar *calendar = [NSCalendar currentCalendar];
NSDate *date = [NSDate date];

NSDateComponents *components = [[NSDateComponents alloc] init];
[components setWeek:1];
[components setHour:12];

NSLog(@"1 week and twelve hours from now: %@", [calendar dateByAddingComponents:components toDate:date options:0]);
~~~

## Creating Dates from Components

Perhaps the most powerful feature of `NSDateComponents`, however, is the ability to go the opposite direction—creating an `NSDate` object from components. `NSCalendar -dateFromComponents:` is the method used for this purpose:

~~~{objective-c}
NSCalendar *calendar = [NSCalendar currentCalendar];

NSDateComponents *components = [[NSDateComponents alloc] init];
[components setYear:1987];
[components setMonth:3];
[components setDay:17];
[components setHour:14];
[components setMinute:20];
[components setSecond:0];

NSLog(@"Awesome time: %@", [calendar dateFromComponents:components]);
~~~

What's particularly interesting about this approach is that a date can be determined by information other than the normal month/day/year approach. So long as a date can be uniquely determined from the provided information, you'll get a result. For example, specifying the year 2013, and the 316th day of the year would return an `NSDate` for 11/12/2013 at midnight (because no time was specified, all time components default to 0).

> Note that passing inconsistent components will either result in some information being discarded, or `nil` being returned.

* * *

`NSDateComponents` and its relationship to `NSCalendar` highlight the distinct advantage having a pedantically-engineered framework like Foundation at your disposal. You may not be doing calendar calculations every day, but when the time comes, knowing how to use `NSDateComponents` will save you eons of frustration.
