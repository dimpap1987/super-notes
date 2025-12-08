- [[Api]]

## Overview
- Transform code to make it harder to understand
- Protect intellectual property
- Prevent reverse engineering
- Reduce code size (minification)

## Obfuscation Techniques

### Name Obfuscation
- Rename classes, methods, variables
- Use meaningless names (a, b, c, _1, _2)
- Preserve public API if needed

### Control Flow Obfuscation
- Add fake branches and dead code
- Flatten control structures
- Insert opaque predicates

### String Encryption
- Encrypt string literals
- Decrypt at runtime
- Hide sensitive strings

### Code Shuffling
- Reorder methods and classes
- Split methods
- Merge methods

## Popular Tools

### ProGuard (Java/Android)
- Shrinking, optimization, obfuscation
- Removes unused code
- Renames classes and methods
- Configuration file-based

```gradle
buildTypes {
    release {
        minifyEnabled true
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
    }
}
```

### JavaScript Obfuscators
- **UglifyJS** - minification and obfuscation
- **JavaScript Obfuscator** - strong obfuscation
- **Terser** - ES6+ minification

```javascript
// Before
function calculateTotal(items) {
    return items.reduce((sum, item) => sum + item.price, 0);
}

// After obfuscation
function _0x1a2b(_0x3c4d,_0x5e6f){return _0x3c4d['reduce']((_0x7g8h,_0x9i0j)=>_0x7g8h+_0x9i0j['price'],0x0);}
```

## Use Cases
- Protect proprietary algorithms
- Prevent code tampering
- Hide API keys and secrets
- Reduce APK/IPA size
- Protect client-side code

## Limitations
- Not 100% secure (determined attackers)
- Can impact debugging
- May affect performance
- Can break reflection-based code
- Maintenance complexity

## Best Practices
- Obfuscate release builds only
- Keep mapping files for debugging
- Test thoroughly after obfuscation
- Don't rely solely on obfuscation for security
- Use code signing for integrity
- Combine with other security measures

## Anti-Patterns
- Obfuscating open-source code
- Obfuscating server-side code unnecessarily
- Breaking functionality for obfuscation
- Not testing obfuscated builds

