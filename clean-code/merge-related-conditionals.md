# Merge Related Conditionals
---

Original Code:

```go
func main() {
  if isMaintenancePeriod {
    log.Fatalf("feature unavailable")
  }
  if !isAuthenticatedUser {
    log.Fatalf("invalid user credentials")
  }
  if !isAuthorizedUser {
    log.Fatalf("user not authorized")
  }
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

Here the conditionals with `isAuthenticatedUser` and `isAuthorizedUser` are relate - they evaluate validity of the user.

So these conditionals can be merged.

Downside is reduced granularity in the log

Updated Code:

```go
func main() {
  if isMaintenancePeriod {
    log.Fatalf("feature unavailable")
  }
  // Both user validity related conditionals have been merged
  if !isAuthenticatedUser || !isAuthorizedUser {
    log.Fatalf("user not authorized")
  }
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
