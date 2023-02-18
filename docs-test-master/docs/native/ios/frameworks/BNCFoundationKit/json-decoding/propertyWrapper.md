PropertyWrapper provide a convenient and safe way to decode json data to mode

- It is easy to declare the specific decoding method
- Compatible with decoding string as the expected type 
- If an error occurred, a default value will be used(DecodableDefaultSource)

the following the the types which conform `DecodableDefaultSource`
- `Optional<T>` : nil
- `Int/Int64/Double/Float/Decimal`: 0
- `string`: nil
- `bool`: false
- `Array<T>`: []
- `Dictionary<Key, Value>`: [:]

## number
- Int
- Int64
- Double
- Float
- Decimal

### Int 

```swift
struct Account : Codable {
    @DecodeIfPresentNoError var numberOfAccounts: Int
    @DecodeIfPresentNoError var numberOfAssets: Int?
}
```

### Int64

```swift
struct Order {
    @DecodeIfPresentNoError var OrderId: Int64
    @DecodeIfPresentNoError var accountId: Int64?
}
```

### Double 
```swift
struct Order : Codable {
    @DecodeIfPresentNoError var createTime: TimeInterval
    @DecodeIfPresentNoError var orderTime: TimeInterval?
}
```

### Float

```swift
struct Example : Codable {
    @DecodeIfPresentNoError var height: Float
    @DecodeIfPresentNoError var width: Float?
}
```

### Decimal

```swift
struct Market : Codable {
    @DecodeIfPresentNoError var open: Decimal
    @DecodeIfPresentNoError var close: Decimal?
}
```

### String

```swift
struct Market : Codable {
    @DecodeIfPresentNoError var baseAsset: String
    @DecodeIfPresentNoError var baseAssetName: String?
}
```

### Bool

```swift
struct Asset : Codable {
    @DecodeIfPresentNoError var etf: Bool
    @DecodeIfPresentNoError var isTest: Bool?
}
```

### Default value

To change the existent default value, declare two properties, the one is `Optional<T>` for JSON decoding, the another one is `Non-Optional` for reading

```swift
struct Account : Codable {
    @DecodeIfPresentNoError private var _numberOfAccounts: Int?
    var numberOfAccounts: Int {
        _numberOfAccounts ?? 1000 // your default value
    }
}
```

`@DecodeIfPresentDefaultTrue` is used to change the default value for `Bool`

```swift
struct Asset : Codable {
    @DecodeIfPresentDefaultTrue var etf: Bool
    @DecodeIfPresentDefaultTrue var isTest: Bool?
}
```

### Struct

```swift
struct Order : Codable {
    
    @DecodeIfPresentNoError var fee: Fee
    
    struct Fee : Codable, DecodableDefaultSource {
         
         @DecodeIfPresentNoError var asset: String
         @DecodeIfPresentNoError var quantity: Decimal
         
         static var defaultValue: Self { 
            .init(asset: .defaultValue, quantity: .defaultValue) // your default value
         }
         
    }
    
}
```

or 

```swift
struct Order : Codable {
    
    @DecodeIfPresentNoError var fee: Fee?

    struct Fee : Codable {
        @DecodeIfPresentNoError var asset: String
        @DecodeIfPresentNoError var quantity: Decimal
    }
    
}
```

### enum

```swift
struct Order : Codable {
    
    @DecodeIfPresentNoError var status: Status

    enum Status : String, Codable, DecodableDefaultSource {
            
        case fulfilled, pending, failed
         
         static var defaultValue: Self { 
            .pending // your default value
         }

    }
    
}
```

or 

```swift
struct Order : Codable {
    
    @DecodeIfPresentNoError var status: Status?

    enum Status : String, Codable {
        case fulfilled, pending, failed
    }
    
}
```

### Date

```swift
struct Order : Codable {
    @DecodeMilliseconds var createTime: Date
    @DecodeMilliseconds var orderTime: Date?
}
```
