# Encryption

Functions for encrypting and decrypting vault data using AES-GCM encryption.

---

## encryptVault()

Encrypts vault data using AES-GCM encryption with password-based key derivation.

### Signature

```typescript
async function encryptVault(
  plainText: string,
  password: string
): Promise<string>;
```

### Parameters

| Parameter   | Type     | Description                      |
| ----------- | -------- | -------------------------------- |
| `plainText` | `string` | The plain text data to encrypt   |
| `password`  | `string` | The password used for encryption |

### Returns

`Promise<string>` - Encrypted data as a string

### Example

```typescript
import { encryptVault } from "safekeys-core";

const vaultData = JSON.stringify({
  id: "vault_123",
  name: "My Vault",
  entries: [
    {
      id: "entry_1",
      title: "GitHub",
      username: "myuser",
      password: "secret123",
    },
  ],
});

const masterPassword = "my-secure-master-password";

try {
  const encryptedData = await encryptVault(vaultData, masterPassword);
  console.log("Vault encrypted successfully");
  console.log("Encrypted data:", encryptedData);
} catch (error) {
  console.error("Encryption failed:", error);
}
```

### Error Handling

```typescript
import { encryptVault } from "safekeys-core";

async function safeEncrypt(data: string, password: string) {
  try {
    const encrypted = await encryptVault(data, password);
    return { success: true, data: encrypted };
  } catch (error) {
    return {
      success: false,
      error: error instanceof Error ? error.message : "Encryption failed",
    };
  }
}
```

---

## decryptVault()

Decrypts vault data using AES-GCM decryption with password-based key derivation.

### Signature

```typescript
async function decryptVault(
  cipherText: string,
  password: string
): Promise<string>;
```

### Parameters

| Parameter    | Type     | Description                      |
| ------------ | -------- | -------------------------------- |
| `cipherText` | `string` | The encrypted data to decrypt    |
| `password`   | `string` | The password used for decryption |

### Returns

`Promise<string>` - Decrypted plain text data

### Example

```typescript
import { decryptVault } from "safekeys-core";

const encryptedData = "..."; // Previously encrypted data
const masterPassword = "my-secure-master-password";

try {
  const decryptedData = await decryptVault(encryptedData, masterPassword);
  const vault = JSON.parse(decryptedData);

  console.log("Vault decrypted successfully");
  console.log("Vault name:", vault.name);
  console.log("Number of entries:", vault.entries.length);
} catch (error) {
  console.error("Decryption failed:", error);
}
```

### Error Handling

```typescript
import { decryptVault } from "safekeys-core";

async function safeDecrypt(encryptedData: string, password: string) {
  try {
    const decrypted = await decryptVault(encryptedData, password);
    return { success: true, data: decrypted };
  } catch (error) {
    return {
      success: false,
      error: error instanceof Error ? error.message : "Decryption failed",
    };
  }
}

// Usage
const result = await safeDecrypt(encryptedData, userPassword);
if (result.success) {
  const vault = JSON.parse(result.data);
} else {
  console.error("Failed to decrypt:", result.error);
}
```

---

## Security Notes

### Password Requirements

- Use strong master passwords (minimum 12 characters)
- Include uppercase, lowercase, numbers, and symbols
- Avoid common passwords or personal information
- Consider using a password manager for the master password

### Best Practices

```typescript
import { encryptVault, decryptVault } from "safekeys-core";

// ✅ Good: Strong password
const strongPassword = "MyVault2024!SecurePassword#123";

// ❌ Bad: Weak password
const weakPassword = "password123";

// ✅ Good: Error handling
async function secureVaultOperation(data: string, password: string) {
  try {
    const encrypted = await encryptVault(data, password);
    // Clear sensitive data from memory
    password = "";
    return encrypted;
  } catch (error) {
    // Clear sensitive data even on error
    password = "";
    throw error;
  }
}

// ✅ Good: Validate password strength before encryption
function validateMasterPassword(password: string): boolean {
  return (
    password.length >= 12 &&
    /[A-Z]/.test(password) &&
    /[a-z]/.test(password) &&
    /[0-9]/.test(password) &&
    /[^A-Za-z0-9]/.test(password)
  );
}
```

### Memory Management

```typescript
// Clear sensitive data after use
let masterPassword = "user-entered-password";
let decryptedData = "";

try {
  decryptedData = await decryptVault(encryptedVault, masterPassword);
  // Use decrypted data...
} finally {
  // Clear sensitive variables
  masterPassword = "";
  decryptedData = "";
}
```

---

## Implementation Details

SafeKeys-Core uses the `crypto-aes-gcm` library for encryption:

- **Algorithm**: AES-256-GCM
- **Key Derivation**: PBKDF2 with SHA-256
- **Authentication**: Built-in with GCM mode
- **Salt**: Randomly generated for each encryption
- **IV**: Randomly generated for each encryption

The encryption process:

1. Generate random salt and IV
2. Derive encryption key from password using PBKDF2
3. Encrypt data using AES-256-GCM
4. Return encrypted data with salt and IV embedded

The decryption process:

1. Extract salt and IV from encrypted data
2. Derive decryption key from password using PBKDF2
3. Decrypt data using AES-256-GCM
4. Verify authentication tag
5. Return plain text data
