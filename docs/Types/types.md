# TypeScript Types

Complete reference for all TypeScript interfaces and enums used in SafeKeys-Core.

---

## Core Interfaces

### VaultEntry

Complete vault entry with all fields and metadata.

```typescript
interface VaultEntry {
  id: string; // Unique identifier
  title: string; // Entry title
  username?: string; // Username/email
  password?: string; // Password
  url?: string; // Website URL
  notes?: string; // Additional notes
  tags?: string[]; // Array of tags
  createdAt: Date; // Creation date
  updatedAt: Date; // Last update date
  favorite?: boolean; // Whether entry is marked as favorite
  category?: EntryCategory; // Entry category
  customFields?: CustomField[]; // Array of custom fields
}
```

#### Example

```typescript
const entry: VaultEntry = {
  id: "entry_1234567890",
  title: "GitHub Account",
  username: "myusername",
  password: "secure-password-123",
  url: "https://github.com",
  notes: "My main GitHub account",
  tags: ["development", "work"],
  favorite: true,
  category: EntryCategory.LOGIN,
  customFields: [],
  createdAt: new Date("2024-01-15T10:30:00.000Z"),
  updatedAt: new Date("2024-01-15T10:30:00.000Z"),
};
```

---

### CustomField

Custom field for additional entry data.

```typescript
interface CustomField {
  id: string; // Unique identifier
  name: string; // Field name
  value: string; // Field value
  type: FieldType; // Field type
  hidden?: boolean; // Whether field is hidden (default: false)
}
```

#### Example

```typescript
const customField: CustomField = {
  id: "field_1234567890",
  name: "API Key",
  value: "sk-1234567890abcdef",
  type: FieldType.PASSWORD,
  hidden: true,
};
```

---

### Vault

Container for organizing vault entries.

```typescript
interface Vault {
  id: string; // Unique identifier
  name: string; // Vault name
  description?: string; // Vault description
  entries: VaultEntry[]; // Array of entries
  createdAt: Date; // Creation date
  updatedAt: Date; // Last update date
  version: string; // Vault format version
  settings?: VaultSettings; // Vault settings
}
```

#### Example

```typescript
const vault: Vault = {
  id: "vault_1234567890",
  name: "Personal Vault",
  description: "My personal passwords and accounts",
  entries: [],
  createdAt: new Date("2024-01-15T10:00:00.000Z"),
  updatedAt: new Date("2024-01-15T10:30:00.000Z"),
  version: "1.0.0",
};
```

---

## Utility Types

### CreateVaultEntryData

Data required to create a new vault entry (without ID and timestamps).

```typescript
type CreateVaultEntryData = Omit<VaultEntry, "id" | "createdAt" | "updatedAt">;
```

#### Example

```typescript
const entryData: CreateVaultEntryData = {
  title: "New Account",
  username: "user@example.com",
  password: "secure-password",
  url: "https://example.com",
  category: EntryCategory.LOGIN,
  tags: ["personal"],
  favorite: false,
};
```

---

### UpdateVaultEntryData

Partial data for updating an existing vault entry.

```typescript
type UpdateVaultEntryData = Partial<Omit<VaultEntry, "id" | "createdAt">>;
```

#### Example

```typescript
const updates: UpdateVaultEntryData = {
  password: "new-secure-password",
  notes: "Updated password on 2024-01-15",
  updatedAt: new Date(),
};
```

---

### ValidationResult

Result object returned by validation functions.

```typescript
interface ValidationResult {
  isValid: boolean; // Whether validation passed
  errors: ValidationError[]; // Array of validation errors
  warnings: ValidationWarning[]; // Array of validation warnings
}

interface ValidationError {
  field: string; // Field that failed validation
  message: string; // Error message
  code: string; // Error code
}

interface ValidationWarning {
  field: string; // Field with warning
  message: string; // Warning message
  code: string; // Warning code
}
```

#### Example

```typescript
const result: ValidationResult = {
  isValid: false,
  errors: [
    {
      field: "title",
      message: "Title is required",
      code: "REQUIRED_FIELD",
    },
  ],
  warnings: [],
};
```

---

## Enums

### EntryCategory

Categories for organizing vault entries.

```typescript
enum EntryCategory {
  LOGIN = "login", // Login credentials
  SECURE_NOTE = "secure_note", // Secure notes
  CREDIT_CARD = "credit_card", // Credit card information
  IDENTITY = "identity", // Identity information
  SOFTWARE_LICENSE = "software_license", // Software licenses
  BANK_ACCOUNT = "bank_account", // Bank account details
  OTHER = "other", // Other/miscellaneous
}
```

#### Example

```typescript
import { EntryCategory } from "safekeys-core";

const entry = createEntry({
  title: "My Bank",
  category: EntryCategory.BANK_ACCOUNT,
});
```

---

### FieldType

Types for custom fields.

```typescript
enum FieldType {
  TEXT = "text", // Plain text
  PASSWORD = "password", // Password (hidden by default)
  EMAIL = "email", // Email address
  URL = "url", // Website URL
  NUMBER = "number", // Numeric value
  DATE = "date", // Date
  TEXTAREA = "textarea", // Multi-line text
}
```

#### Example

```typescript
import { FieldType } from "safekeys-core";

const customField = createCustomField(
  "Recovery Email",
  "recovery@example.com",
  FieldType.EMAIL
);
```

---