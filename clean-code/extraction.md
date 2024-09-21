# Extraction
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

It is difficult to figure out what the purpose of these blocks are.

If we extract them to separate functions with names that explain the motive, that makes the objective very clear to the developer.

```go
func isValidUser() {
  if !isAuthenticatedUser {
    log.Fatalf("invalid user credentials")
  }
  if !isAuthorizedUser {
    log.Fatalf("user not authorized")
  }
}

func calculateTaxes(product product) {
  switch product.Type() {
    case "alcohol":
      total += alcoholTax
    case "electronics":
      total += electronicsTax
    default:
      total += generalTax
  }
}

func main() {
  if isMaintenancePeriod {
    log.Fatalf("feature unavailable")
  }
  // Clearly shows that the user is being validated
  isValidUser()
  // Clearly shows that for each product in cart,
  // Tax is being calculated
  for _, product := range cart {
    calculateTaxes(product)
  }
}
```

Now the motive of the `main` function is so easily understandable
