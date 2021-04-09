# Grammar Documentation for node type "NumberRule"

NumberRules are rules that apply on values which have the data type "Number", "Integer" or "Float" from Schema.org.

The specific rule type is given by the value in the "$rule" property. The properties which rules can have are explained in the Meta\_Rules.md document. Note that the "subject" property can be omitted, if the "$path" object has an empty JSON Pointer as value \(see Meta\_Rules.md for details\).

However, the data type for the "parameter" depends on the "$rule" value \(e.g. arguments for the rule function\), and is explained in the following.

![Syntax diagram](../../../../.gitbook/assets/Rule.png)

## matchesPattern

The "matchesPattern" rule checks if a given number \(the validated subject\) matches a given numeric pattern.

```javascript
{
  "$type": "NumberRule",
  "$rule": "matchesPattern",
  "subject": {
      "$path": "/starRating/ratingValue"
  },
  "parameter": "0-5"
}
```

The parameter is a numeric pattern \(see Meta\_NumericPattern.md\) that serves as the second argument for the rule function. The given number \(the validated subject\) must match this pattern.

## isInSet

The "isInSet" rule checks if a given number \(the validated subject\) is in the given set of allowed values.

```javascript
{
  "$type": "NumberRule",
  "$rule": "isInSet",
  "parameter": [1,2.5,3.77,5,10]
}
```

The parameter" is an array which contains numbers, which are valid instances for the given number \(the validated subject\).

## isInteger

The "isInteger" rule checks if a given number \(the validated subject\) is an Integer or not. The outcome must match a given boolean. There is no definition about what an "Integer" is at [https://schema.org/Integer](https://schema.org/Integer) . For the domain specification context, we handle a number as an Integer, if and only if the number has no floating point. e.g. 1 is an Integer, 1. and 1.0 are not Integers

```javascript
{
  "$type": "NumberRule",
  "$rule": "isInteger",
  "parameter": true
}
```

The parameter is a boolean, that must match the outcome of the Integer check to fulfill the constraint.

## isFloat

The "isFloat" rule checks if a given number \(the validated subject\) is a Float or not. The outcome must match a given boolean. There is no definition about what an "Float" is at [https://schema.org/Float](https://schema.org/Float) . For the domain specification context, we handle a number as a Float, if and only if the number has a floating point. e.g. 1. and 1.0 are Floats, 1 is not

```javascript
{
  "$type": "NumberRule",
  "$rule": "isFloat",
  "parameter": false
}
```

The parameter is a boolean, that must match the outcome of the Float check to fulfill the constraint.

## hasDigitsLength

The "hasDigitsLength" rule checks the digit amount of a given number \(the validated subject\), that matches a given numeric pattern.

```javascript
{
  "$type": "NumberRule",
  "$rule": "hasDigitsLength",
  "parameter": "<=5"
}
```

The parameter is a numeric pattern \(see Meta\_NumericPattern.md\) that serves as the second argument for the rule function. The amount of digits must match the pattern.

The number of digits for Floats omit the floating point. -&gt; String\(VALUE\).replace\('.', ''\).length

## hasDecimalDigitsLength

The "hasDecimalDigitsLength" rule checks the decimal digit amount of a given number \(the validated subject\), that matches a given numeric pattern.

```javascript
{
  "$type": "NumberRule",
  "$rule": "hasDecimalDigitsLength",
  "parameter": "2"
}
```

The parameter is a numeric pattern \(see Meta\_NumericPattern.md\) that serves as the second argument for the rule function. The amount of digits after the floating point must match the pattern.

Integers are supposed to have 0 decimal digits.

