# Grammar Documentation for node type "DateTimeRule"

```text
DateTime: A combination of date and time in ISO 8691 date and time format. 

Schema.org demands the format: 
[-]YYYY-MM-DDThh:mm:ss[Z|(+|-)hh:mm] 

which is a concatenation of four elements: 
Date + “T” + Time + Timezone. 

Date: [-]YYYY-MM-DD , YYYY being the year, MM being the month and DD being the day. 

The letter “T” is just a separation indicator between Date and Time. 

Time: hh:mm:ss , hh being the hour in 24-hour timekeeping system, mm being the minutes and ss being the seconds. 

The Timezone can be unconsidered (not written at all) 
or considered but not specified (the letter “Z” is written) 
or stated in the format: (+|-)hh:mm , hh being the hour in 24-hour timekeeping system, mm being the minutes.
```

DateTimeRules are rules that apply on values which have the data type "DateTime" from Schema.org.

The specific rule type is given by the value in the "$rule" property. The properties which rules can have are explained in the Meta\_Rules.md document. Note that the "subject" property can be omitted, if the "$path" object has an empty JSON Pointer as value \(see Meta\_Rules.md for details\).

However, the data type for the "parameter" depends on the "$rule" value \(e.g. arguments for the rule function\), and is explained in the following.

![Syntax diagram](../../../../.gitbook/assets/Rule.png)

## equals

The "equals" rule checks if a given DateTime value \(the validated value\) matches a given DateTime.

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "equals",
  "subject": {
      "$path": "/checkoutTime"
  },
  "parameter": "2018-04-20T13:37:00"
}
```

The parameter is a DateTime which serves as the second argument for the rule function. The validated subject must be the same as the given parameter.

## isBefore

The "isBefore" rule checks if a given DateTime value \(the validated value\) is chronologically before a given DateTime.

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "isBefore",
  "parameter": "2018-04-25T22:00:00"
}
```

The parameter is a DateTime which serves as the second argument for the rule function. The validated subject must be chronologically before the given parameter.

## isAfter

The "isAfter" rule checks if a given DateTime \(the validated value\) is chronologically after a given DateTime.

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "isAfter",
  "parameter": "2015-02-05T09:00:00Z"
}
```

The parameter is a DateTime which serves as the second argument for the rule function. The validated subject must be chronologically after the given parameter.

## isInSet

The "isInSet" rule checks if a given DateTime \(the validated value\) is in the given set of allowed values.

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "isInSet",
  "parameter": [
    "2014-04-05T09:00:00",
    "2014-04-06T10:00:00Z",
    "2014-04-07T11:30:00"
  ],
  "description": "The DateTime must match one of the given values"
}
```

The parameter is an array which contains DateTimes, which are valid instances for a given DateTime \(the validated subject\).

## hasYear

The "hasYear" rule checks if a given DateTime \(the validated value\) has a value for the year, that is conform to a given numeric pattern.

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "hasYear",
  "parameter": ">2015",
  "description": "Year of the datetime must be later than 2015."
}
```

The parameter is a numeric pattern, that has to be matched by the year of a given DateTime \(the validated subject\).

## hasMonth

The "hasMonth" rule checks if a given DateTime \(the validated value\) has a value for the month, that is conform to a given numeric pattern.

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "hasMonth",
  "parameter": "1-3",
  "description": "The month of the datetime must match the numeric pattern -> Month must be between 1 and 3."
}
```

The parameter is a numeric pattern, that has to be matched by the month of a given DateTime \(the validated subject\).

## hasDay

The "hasDay" rule checks if a given DateTime \(the validated value\) has a value for the day, that is conform to a given numeric pattern.

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "hasDay",
  "parameter": "(15 | 30)",
  "description": "The day of the datetime must match the numeric pattern -> Day must be either 15 or 30."
}
```

The parameter is a numeric pattern, that has to be matched by the day of a given DateTime \(the validated subject\).

## hasHour

The "hasHour" rule checks if a given DateTime \(the validated value\) has a value for the hour, that is conform to a given numeric pattern.

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "hasHour",
  "parameter": "(>7 & < 18)"
}
```

The parameter is a numeric pattern, that has to be matched by the hour of a given DateTime \(the validated subject\).

## hasMinutes

The "hasMinutes" rule checks if a given DateTime \(the validated value\) has a value for the minutes, that is conform to a given numeric pattern.

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "hasMinutes",
  "parameter": "(0 | 15 | 30 | 45)"
}
```

The parameter is a numeric pattern, that has to be matched by the minutes of a given DateTime \(the validated subject\).

## hasSeconds

The "hasSeconds" rule checks if a given DateTime \(the validated value\) has a value for the seconds, that is conform to a given numeric pattern.

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "hasSeconds",
  "parameter": "0"
}
```

The parameter is a numeric pattern, that has to be matched by the seconds of a given DateTime \(the validated subject\).

## hasTimezone

The "hasTimezone" rule checks if a given DateTime \(the validated value\) has a value for the timezone, that matches a given value with format: \(+\|-\)hh:mm

```javascript
{
  "$type": "DateTimeRule",
  "$rule": "hasTimezone",
  "parameter": "+02:00"
}
```

The parameter is a timezone string with format \(+\|-\)hh:mm that has to be matched by the timezone of a given DateTime \(the validated subject\). "Z" or a not defined timezone are handled as a not fulfilled constraint.

