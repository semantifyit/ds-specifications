# Rules Grammar

The purpose of rules is the semantic verification of values.

Rules are tied to the Schema.org data types on which they can apply.

Since there are nodes for data types inside the DS grammar, it is easy to verify if a given rule for that node CAN apply to the specified data type. e.g.

```javascript
{
  "$type": "DataType",
  "$dataType": "Date",
  "rules": []
}
```

Inside the "rules" array, there are supposed to be only rules for the data type "Date" \(or ComplexRules which combine other DateRules\), e.g.

```javascript
{
  "$type": "DateRule",
  "$rule": "isAfter",
  "subject": {
    "$path": ""
  },
  "parameter": "2015-03-20"
}
```

Note that the "subject" property always has a path-object as value. That path-object is a relative reference to the value to verify \(path inside the annotation\). An empty path means the rule applies to the actual relative path of the verification; this is expected for all rules that apply on a DataType, since the semantic check concerns THAT referenced data type \(a terminal value of the annotation\). In those cases it is possible to omit the "subject" property, which will be handled as if there was an empty path-node. e.g.

```javascript
{
  "$type": "DateRule",
  "$rule": "isAfter",
  "parameter": "2015-03-20"
}
```

The nested object nature of JSON and JSON-LD documents allows to pass corresponding nested objects further to the verifier and execute the verification process with a divide-and-conquer strategy. The $path inside a rule \(in a Domain Specification\) is a relative JSON Pointer starting at the position of the actual nested object \(from the Annotation\) that is verified.

Consider the following Annotation "Ann":

```javascript
{
  "@context": "http://schema.org",
  "@type": "Hotel",
  "name": "ACME Hotel Innsbruck",
  "description": "A beautifully located business hotel right in the heart of the alps. Watch the sun rise over the scenic Inn valley while enjoying your morning coffee.",
  "address": {
    "@type": "PostalAddress",
    "addressCountry": "Austria",
    "addressLocality": "Innsbruck",
    "addressRegion": "Tyrol",
    "postalCode": "6020",
    "streetAddress": "Technikerstrasse 21"
  },
  "location": {
    "@type": "PostalAddress",
    "addressCountry": "Austria"
  },
  "telephone": "+43 512 8000-0",
  "photo": "http://www.acme-innsbruck.at//media/hotel_front.png",
  "starRating": {
    "@type": "Rating",
    "ratingValue": "4"
  },
  "priceRange": "$100 - $240"
}
```

Imagine a Domain Specification "DS". DS has \(among others\)

* a RestrictedClass node for the "Hotel" object of the Annotation "Ann"
* a RestrictedClass node for the "PostalAddress" object for the property "address" of the Hotel
* and a DataType node for the "Austria" value for the property "addressCountry" of the PostalAddress

Each of these nodes could hold a TextRule to restrict the value "Austria" for the property addressCountry. Just the JSON Pointer for the subject path would be different, e.g.

\(Watch out for the "$path" values\)

A rule inside the DataType node for the property "addressCountry" would look like this:

```javascript
{
  "$type": "TextRule",
  "$rule": "equals",
  "subject": {
    "$path": ""
  },
  "parameter": "Austria"
}
```

A rule inside the RestrictedClass for the "PostalAddress" object would look like this:

```javascript
{
  "$type": "TextRule",
  "$rule": "equals",
  "subject": {
    "$path": "/addressCountry"
  },
  "parameter": "Austria"
}
```

A rule inside the RestrictedClass for the "Hotel" object would look like this:

```javascript
{
  "$type": "TextRule",
  "$rule": "equals",
  "subject": {
    "$path": "/address/addressCountry"
  },
  "parameter": "Austria"
}
```

As shown, RestrictedClass nodes can also have rules, and they are supposed to compare values in different sub-parts of the annotation, hence compare values of different properties. The following rule is inside the RestrictedClass for the "Hotel" object; it compares the addressCountry of the "address" property with the addressCountry of the "location" property. They must be the same:

```javascript
{
  "$type": "TextRule",
  "$rule": "equals",
  "subject": {
    "$path": "/address/addressCountry"
  },
  "parameter": {
    "$path": "/location/addressCountry"
  }
}
```

Another example \(IF the Hotel is in "Austria", THEN the telephone number must start with "+43"\)

```javascript
{
  "$type": "ComplexRule",
  "$rule": "ifThen",
  "ifRules": [
    {
      "$type": "TextRule",
      "$rule": "equals",
      "subject": {
        "$path": "/address/addressCountry"
      },
      "parameter": "Austria"
    }
  ],
  "thenRules": [
    {
      "$type": "TextRule",
      "$rule": "startsWith",
      "subject": {
        "$path": "/telephone"
      },
      "parameter": "+43"
    }
  ]
}
```

The value for the "parameter" can have different types, depending on the rule. As shown in the previous example, this value can also be referenced from the verified annotation with a JSON Pointer of a "$path" object. It can also be a referenced value from the Domain Specification with a JSON Pointer in a "$ref" object. Note that the JSON Pointer in "$ref" is always relative to the whole domain specification, whereas the JSON Pointer in "$path" is always relative to the actual path of the verified annotation sub-object!

Definition of the value "countryValue" for the whole Domain Specification:

```javascript
 {
  "$type": "DomainSpecification",
  "$schema": "http://ontologies.sti-innsbruck.at/dsv-json/1.0/",
  "$SDOversion": "http://schema.org/version/3.4/",
  "name": "Austrian Hotel",
  "definitions": {
    "countryValue": "Austria"
  }
  ...
}
```

The defined "countryValue" can be used in a rule with a "$ref" object, e.g.

```javascript
{
  "$type": "TextRule",
  "$rule": "equals",
  "subject": {
    "$path": "/address/addressCountry"
  },
  "parameter": {
    "$ref": "/definitions/countryValue"
  }
}
```

## Verification behavior

The expected behavior of a verification tool checking if a rule is violated or not, is the following:

1. All rules are checked on their own. Depending on the verification of each rule an error report is created. For each rule:
2. Check if the rule is applicable: The rule type in "$type" of the rule node must match the data type of the value which is checked.
   * a\) If the Rule is not applicable \(e.g. a TextRule for a boolean value\) -&gt; verification result of the rule is null \(not true nor false, since the rule can't be checked\).
   * b\) If the Rule is applicable -&gt; continue with next step
3. The verified value is checked with a built-in function that implements the rule in "$rule". Arguments for the function are the verified value, and arguments for the rule. \(e.g. "value": "2015-03-20" in the previous example\).
   * a\) The value is conform to the rule: verification result of the rule is true.
   * b\) The value is not conform to the rule: verification result of the rule is false. An error report is generated for this rule \(see error report grammar\).
   * c\) There is an procedural error during the verification: verification result of the rule is false. An error report is generated with an explanation about the process error, it is expected that the input values of the arguments for the rule function are invalid/cause the problem.

## Rule categories

The "$type" of rules are:

* TextRule, applies only to text values according to SDO data types \(Text, URL\)
* BooleanRule, applies only to boolean values according to SDO data types \(Boolean\)
* DateRule, applies only to date values according to SDO data types \(Date\)
* TimeRule, applies only to Time values according to SDO data types \(Time\)
* DateTimeRule, applies only to DateTime values according to SDO data types \(DateTime\)
* NumberRule, applies only to numberic values accoarding to SDO data types \(Number, Integer, Float\)
* ComplexRule, applies to any value/object to include logical checks into the verification process

| Schema.org Type | Applicable Rule Type |
| :--- | :--- |
| Text | TextRule |
| URL | TextRule |
| Boolean | BooleanRule |
| Number | NumberRule |
| Integer | NumberRule |
| Float | NumberRule |
| Date | DateRule |
| Time | TimeRule |
| DateTime | DateTimeRule |
| All | ComplexRule |

Note that classes and restricted classes can also have semantic rules. However, these rules must point to a value inside the verified object with a "$path" value, to indicate which part of the object should be verified. Such a rule can only apply if the indicated value matches the applicable rule type \(table above\).

The previous categorization is based on the functionality, and argument data types which are expected for the different rules. It makes sense to compare a Date to a Date, but not a Time to a Date. For numbers anything makes sense, and can be compared, e.g. if integer 3 is bigger than float 2.453

Note that Schema.org allows the usage of strings as representation for any data type. A system verifying Schema.org annotations SHOULD encourage users to use the native data types instead of their string representation, but SHOULD allow their usage anyway. e.g. "true" and true should be valid boolean instances and checked for any BooleanRules which may be applied.

## Properties of a Rule object

Rules may have additional properties which are specific to **meta information**.

```javascript
{
  "$type": "TextRule",
  "$rule": "hasLength",
  "parameter": "20",
  "name": "Length Rule",
  "description": "String must have 20 characters."
}
```

Update: Removed optional properties regarding error handling. It is true that custom error handling is more interesting for semantic rules than for the rest of the verification, but after working on the verification Report and Error vocabulary, I think that the "custom error information" should not be part of the core DS vocabulary; it can easily be defined as an extension on top of the regular verification process.

## List of Rules

The built-in rules of the DS vocabulary are specified in the following table, see documentation of each rule type for more details. Note that there are types of rules \($type\), which are bound to the data types of Schema.org, and that there are sub-types of rules \($rule\), specifying the actual purpose of that rule. The decision for this modeling is based on following expectations:

* Increased readability.
* Having own categories for the "$type" makes it easier to check if a that rule category applies to the data type of a given value.
* Naming of "$rule" values can be reused for the different $types \(e.g. "equals"\) without causing confusion.
* The grammar for "$type" is not overloaded by the large amount of rules types

The values for "$rule", hence the rule names, are written in camelCase style describing a verb. e.g. "hasLength", "hasFormat", "isInSet"

Rules Table \("entity" refers to the verified entity\):

| $type | $rule | parameter datatype | description |
| :--- | :--- | :--- | :--- |
| TextRule | hasLength | numericPattern | Length of entity \(string\) is conform to given numeric pattern. |
| TextRule | startsWith | string | Entity \(string\) starts with a given sub-string. |
| TextRule | endsWith | string | Entity \(string\) ends with a given sub-string. |
| TextRule | contains | string | Entity \(string\) contains a given sub-string. |
| TextRule | equals | string | Entity \(string\) is equal to a given string. |
| TextRule | isInSet | Array of strings | Entity \(string\) is equal to a value, which is in a given set of valid strings. |
| TextRule | matchesPattern | regex | Entity \(string\) matches a given Regex pattern. |
| TextRule | hasFormat | format | Entity \(string\) has a given format. |

| $type | $rule | parameter datatype | description |
| :--- | :--- | :--- | :--- |
| BooleanRule | equals | boolean | Entity \(boolean\) is equal to a given boolean. |

| $type | $rule | parameter datatype | description |
| :--- | :--- | :--- | :--- |
| DateRule | equals | Date | Entity \(Date\) is equal to a given date. |
| DateRule | isInSet | Array of Dates | Entity \(Date\) is equal to a value, which is in a given set of valid Dates. |
| DateRule | isbefore | Date | Entity \(Date\) is before a given Date. |
| DateRule | isafter | Date | Entity \(Date\) is after a given Date. |
| DateRule | hasYear | numericPattern | Year of the Entity \(Date\) is conform to given numeric pattern. |
| DateRule | hasMonth | numericPattern | Month of the Entity \(Date\) is conform to given numeric pattern. |
| DateRule | hasDay | numericPattern | Day of the Entity \(Date\) is conform to given numeric pattern. |

| $type | $rule | parameter datatype | description |
| :--- | :--- | :--- | :--- |
| TimeRule | equals | Time | Entity \(Time\) is equal to a given time. |
| TimeRule | isInSet | Array of Times | Entity \(Time\) is equal to a value, which is in a given set of valid Times. |
| TimeRule | isbefore | Time | Entity \(Time\) is before a given Time. |
| TimeRule | isafter | Time | Entity \(Time\) is after a given Time. |
| TimeRule | hasHour | numericPattern | Hour of the Entity \(Time\) is conform to given numeric pattern. |
| TimeRule | hasMinutes | numericPattern | Minutes of the Entity \(Time\) is conform to given numeric pattern. |
| TimeRule | hasSeconds | numericPattern | Seconds of the Entity \(Time\) is conform to given numeric pattern. |
| TimeRule | hasTimezone | Timezone | Timezone of the Entity \(Time\) has a timezone which matches a given value with format: \(+/-\)hh:mm |

| $type | $rule | parameter datatype | description |
| :--- | :--- | :--- | :--- |
| DateTimeRule | equals | DateTime | Entity \(DateTime\) is equal to a given DateTime. |
| DateTimeRule | isInSet | Array of DateTimes | Entity \(DateTime\) is equal to a value, which is in a given set of valid dateTimes. |
| DateTimeRule | isbefore | DateTime | Entity \(DateTime\) is before a given DateTime. |
| DateTimeRule | isafter | DateTime | Entity \(DateTime\) is after a given DateTime. |
| DateTimeRule | hasYear | numericPattern | Year of the Entity \(DateTime\) is conform to given numeric pattern. |
| DateTimeRule | hasMonth | numericPattern | Month of the Entity \(DateTime\) is conform to given numeric pattern. |
| DateTimeRule | hasDay | numericPattern | Day of the Entity \(DateTime\) is conform to given numeric pattern. |
| DateTimeRule | hasHour | numericPattern | Hour of the Entity \(DateTime\) is conform to given numeric pattern. |
| DateTimeRule | hasMinutes | numericPattern | Minutes of the Entity \(DateTime\) is conform to given numeric pattern. |
| DateTimeRule | hasSeconds | numericPattern | Seconds of the Entity \(DateTime\) is conform to given numeric pattern. |
| DateTimeRule | hasTimezone | Timezone | Timezone of the Entity \(DateTime\) has a timezone which matches a given value with format: \(+/-\)hh:mm |

| $type | $rule | parameter datatype | description |
| :--- | :--- | :--- | :--- |
| NumberRule | matchesPattern | numericPattern | Entity \(Number\) matches a given numeric pattern. |
| NumberRule | isInSet | Array of Numbers | Entity \(Number\) is equal to a value, which is in a given set of valid Numbers. |
| NumberRule | isInteger | Boolean | Entity \(Number\) is an Integer \(has no decimal value/has no floating point\). |
| NumberRule | isFloat | Boolean | Entity \(Number\) is a Float \(has a floating point\) |
| NumberRule | hasDigitsLength | numericPattern | Entity \(Number\) has an amount of digits that matches a given numeric pattern. |
| NumberRule | hasDecimalDigitsLength | numericPattern | Entity \(Number\) has an amount of decimal digits that matches a given numeric pattern. |

| $type | $rule | parameters with datatypes | description |
| :--- | :--- | :--- | :--- |
| ComplexRule | and | rules \(Array of rule objects\) | "And" from boolean logic: All rules inside the "rules" array must pass the check. |
| ComplexRule | or | rules \(Array of rule objects\) | "Or" from boolean logic: At least one of the rules inside the "rules" array must pass the check. |
| ComplexRule | not | rules \(Array of rule objects\) | "Not" from boolean logic: Not a single of the rules inside the "rules" array must pass the check. |
| ComplexRule | ifThen | ifRules \(Array of rule objects\) thenRules \(Array of rule objects\) | A conditional constraint from causal logic: If all the rules inside the "ifRules" array pass the check, then all the rules inside the "thenRules" array must also pass the check. |

