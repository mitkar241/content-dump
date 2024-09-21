# Conditional Inversion
---

Original Program:

```go
func main() {
  if !isMaintenancePeriod {
    if isAuthenticatedUser {
      if isAuthorizedUser {
        for _, product := range cart {
          switch product.Type() {
            case "alcohol":
              total += alcoholTax
            case "electronics":
              total += electronicsTax
            default:
              total += generalTax
          }
        }
      } else {
        log.Fatalf("user not authorized")
      }
    } else {
      log.Fatalf("invalid user credentials")
    }
  } else {
    log.Fatalf("feature unavailable")
  }
}
```

This is an example of a deeply nested program.

Logic like this is difficult to follow because it is difficult to keep track of what are the conditions that allow the code flow to traverse.

Solution: Inversion of Conditionals

If we invert the condition in the conditional, it collapses the nested structure by one level

```go
func main() {
  // Inverted Conditional
  if isMaintenancePeriod {
    log.Fatalf("feature unavailable")
  }
  // Inverted Conditional
  if !isAuthenticatedUser {
    log.Fatalf("invalid user credentials")
  }
  // Inverted Conditional
  if !isAuthorizedUser {
    log.Fatalf("user not authorized")
  }
  // No need to consider code before this point
  // as it wont affect code flow afterwards
  for _, product := range cart {
    switch product.Type() {
      case "alcohol":
        total += alcoholTax
      case "electronics":
        total += electronicsTax
      default:
        total += generalTax
    }
  }
}
```
