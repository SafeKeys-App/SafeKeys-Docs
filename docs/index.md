# SafeKeys-Core Documentation

<p align="center">
<strong>🔐 TypeScript Library for Password Management</strong>
</p>

<p align="center">
<a href="https://github.com/SafeKeys-App/SafeKeys-Core"><img src="https://img.shields.io/github/stars/SafeKeys-App/SafeKeys-Core?style=social" alt="GitHub Stars" style="height: 28px; margin: 0 8px;"></a>
<a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT" style="height: 28px; margin: 0 8px;"></a>
<a href="http://www.typescriptlang.org/"><img src="https://img.shields.io/badge/%3C%2F%3E-TypeScript-%230074c1.svg" alt="TypeScript" style="height: 28px; margin: 0 8px;"></a>
</p>

---

## 📚 Documentation

<div class="grid cards" markdown>

- 🔐 **Encryption Functions**

  ***

  `encryptVault()` and `decryptVault()` functions

  [:octicons-arrow-right-24: View Functions](encryption.md)

- 📦 **Entry Management**

  ***

  `createEntry()`, `updateEntry()`, and related functions

  [:octicons-arrow-right-24: View Functions](entry-management.md)

- ✅ **Validation**

  ***

  Validation functions and schemas

  [:octicons-arrow-right-24: View Functions](validation.md)

- 🔧 **TypeScript Types**

  ***

  Interfaces, enums, and type definitions

  [:octicons-arrow-right-24: View Types](types.md)

</div>

---

## 🚀 Quick Example

```typescript
import { encryptVault, decryptVault, createEntry } from "safekeys-core";

// Create a password entry
const entry = createEntry({
  title: "GitHub",
  username: "myusername",
  password: "secure-password-123",
  url: "https://github.com",
});

// Encrypt the data
const encrypted = await encryptVault(JSON.stringify(entry), "master-password");

// Decrypt when needed
const decrypted = await decryptVault(encrypted, "master-password");
```

---

## 🔗 Links

- **[GitHub Repository](https://github.com/SafeKeys-App/SafeKeys-Core)** - Source code
- **[npm Package](https://www.npmjs.com/package/safekeys-core)** - Install via npm
