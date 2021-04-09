# Numeric Pattern

**A numeric pattern is a string which defines a valid set of numeric values**.

Such a numeric pattern is used in the DS vocabulary to define constraint on numeric values, the length of strings, or the allowed cardinality of properties.

Integers and Float values \(simple float without exponent letter\) can be used in numeric patterns. Spaces between characters are possible, but not necessary.

Expressions for this pattern include \(Val stands for a value, Expr stands for an expression\):

\(note: because of markdown limitations the examples in table contain ⎮ instead of \| \(pipe\) as symbol for "or" \)

| Expression | pattern syntax |
| :--- | :--- |
| Number | Val1 |
| Range | Val1 - Val2 |
| More than excl. | &gt; Val1 |
| Less than excl. | &lt; Val1 |
| More than incl. | &gt;= Val1 |
| Less than incl. | &lt;= Val1 |
| MultipleOf | % Val1 |
| Or | \(Expr1 ⎮ Expr2 ⎮ ... ⎮ ExprN\) |
| And | \(Expr1 & Expr2 & ... & ExprN\) |
| Not | !Expr1 |

Examples:

| Numeric pattern | description |
| :--- | :--- |
| 20 | equal 20 |
| &lt;4 | smaller than 4 |
| &gt;=5 | bigger or equal 5 |
| %4 | multiple of 4 |
| 1-3 | between 1 and 3 |
| \(0 ⎮ 1\) | 0 or 1 |
| \(&gt;1 & &lt;10\) | bigger than 1 and smaller than 10 |
| !0 | not 0 |
| 1-1.337 | between 1 and 1.337 |
| \(\(&gt;2 & 9000\) | multiple of 2 and less than 10, or multiple of 3, or bigger than 9000 |

## Syntax diagram for numeric pattern

### NumericPattern

![Syntax diagram](../../../.gitbook/assets/NumericPattern.png)

### NumberExpression

![Syntax diagram](../../../.gitbook/assets/NumericPattern_NumberExpression.png)

### RangeExpression

![Syntax diagram](../../../.gitbook/assets/NumericPattern_RangeExpression.png)

### MoreThanExclExpression

![Syntax diagram](../../../.gitbook/assets/NumericPattern_MoreThanExclExpression.png)

### LessThanExclExpression

![Syntax diagram](../../../.gitbook/assets/NumericPattern_LessThanExclExpression.png)

### MoreThanInclExpression

![Syntax diagram](../../../.gitbook/assets/NumericPattern_MoreThanInclExpression.png)

### LessThanInclExpression

![Syntax diagram](../../../.gitbook/assets/NumericPattern_LessThanInclExpression.png)

### MultipleOfExpression

![Syntax diagram](../../../.gitbook/assets/NumericPattern_MultipleOfExpression.png)

### OrExpression

![Syntax diagram](../../../.gitbook/assets/NumericPattern_OrExpression.png)

### AndExpression

![Syntax diagram](../../../.gitbook/assets/NumericPattern_AndExpression.png)

### NotExpression

![Syntax diagram](../../../.gitbook/assets/NumericPattern_NotExpression.png)

