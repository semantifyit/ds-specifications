# Grammar Documentation for node type "TimeRule"

```text
Time: A time value in ISO 8691 date format. 
Schema.org demands the format hh:mm:ss[Z|(+|-)hh:mm] which is a concatenation of:
Time + Timezone. 

Time:  hh:mm:ss , hh being the hour in 24-hour timekeeping system, mm being the minutes and ss being the seconds. 

The Timezone can be unconsidered (not written at all) 
or considered but not specified (the letter “Z” is written)
or stated in the format: 
(+|-)hh:mm , hh being the hour in 24-hour timekeeping system, mm being the minutes.
```

TimeRules are rules that apply on values which have the data type "Time" from Schema.org.

The specific rule type is given by the value in the "$rule" property. The properties which rules can have are explained in the Meta\_Rules.md document. Note that the "subject" property can be omitted, if the "$path" object has an empty JSON Pointer as value \(see Meta\_Rules.md for details\).

However, the data type for the "parameter" depends on the "$rule" value \(e.g. arguments for the rule function\), and is explained in the following.

![Syntax diagram](../../../../.gitbook/assets/Rule.png)

## equals

The "equals" rule checks if a given Time value \(the validated subject\) matches a given Time.

```javascript
{
  "$type": "TimeRule",
  "$rule": "equals",
  "subject": {
      "$path": "/closes"
  },
  "parameter": "04:00:00"
}
```

The parameter is a Time which serves as the second argument for the rule function. The validated subject must be the same as the given parameter.

## isBefore

The "isBefore" rule checks if a given Time value \(the validated subject\) is chronologically before a given Time.

```javascript
{
  "$type": "TimeRule",
  "$rule": "isBefore",
  "parameter": "20:30:00+02:00"
}
```

The parameter is a Time which serves as the second argument for the rule function. The validated subject must be chronologically before the given parameter.

## isAfter

The "isAfter" rule checks if a given Time \(the validated subject\) is chronologically after a given Time.

```javascript
{
  "$type": "TimeRule",
  "$rule": "isAfter",
  "parameter": "08:00:00Z"
}
```

The parameter is a Time which serves as the second argument for the rule function. The validated subject must be chronologically after the given parameter.

## isInSet

The "isInSet" rule checks if a given Time \(the validated subject\) is in the given set of allowed values.

```javascript
{
  "$type": "TimeRule",
  "$rule": "isInSet",
  "parameter": [
    "09:00:00",
    "09:00:00Z",
    "09:00:00+01:00"
  ],
  "description": "The Time must match one of the given values"
}
```

The parameter is an array which contains Times, which are valid instances for a given Time \(the validated subject\).

## hasHour

The "hasHour" rule checks if a given Time \(the validated subject\) has a value for the hour, that is conform to a given numeric pattern.

```javascript
{
  "$type": "TimeRule",
  "$rule": "hasHour",
  "parameter": "(>7 & < 18)"
}
```

The parameter is a numeric pattern, that has to be matched by the hour of a given Time \(the validated subject\).

## hasMinutes

The "hasMinutes" rule checks if a given Time \(the validated subject\) has a value for the minutes, that is conform to a given numeric pattern.

```javascript
{
  "$type": "TimeRule",
  "$rule": "hasMinutes",
  "parameter": "(0 | 15 | 30 | 45)"
}
```

The parameter is a numeric pattern, that has to be matched by the minutes of a given Time \(the validated subject\).

## hasSeconds

The "hasSeconds" rule checks if a given Time \(the validated subject\) has a value for the seconds, that is conform to a given numeric pattern.

```javascript
{
  "$type": "TimeRule",
  "$rule": "hasSeconds",
  "parameter": "0"
}
```

The parameter is a numeric pattern, that has to be matched by the seconds of a given Time \(the validated subject\).

## hasTimezone

The "hasTimezone" rule checks if a given Time \(the validated value\) has a value for the timezone, that matches a given value with format: \(+\|-\)hh:mm

```javascript
{
  "$type": "TimeRule",
  "$rule": "hasTimezone",
  "parameter": "+02:00"
}
```

The parameter is a timezone string with format \(+\|-\)hh:mm that has to be matched by the timezone of a given Time \(the validated subject\). "Z" or a not defined timezone are handled as a not fulfilled constraint.

