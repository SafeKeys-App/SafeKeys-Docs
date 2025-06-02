# Validation Functions

Functions for validating data and schemas in SafeKeys-Core.

---

## validateCreateEntryData()

Validates data before creating a new vault entry.

### Signature

```typescript
function validateCreateEntryData(data: CreateVaultEntryData): ValidationResult;
```

### Parameters

| Parameter | Type                   | Description                 |
| --------- | ---------------------- | --------------------------- |
| `data`    | `CreateVaultEntryData` | The data object to validate |

### Returns

`ValidationResult` - Validation result with errors and warnings

### Example

```typescript
import { validateCreateEntryData, EntryCategory } from "safekeys-core";

const entryData = {
  title: "GitHub",
  username: "myuser",
  password: "secret123",
  url: "https://github.com",
  category: EntryCategory.LOGIN,
};

const result = validateCreateEntryData(entryData);

if (result.isValid) {
  console.log("Data is valid");
  // Proceed with createEntry(entryData)
} else {
  console.log("Validation errors:", result.errors);
  console.log("Validation warnings:", result.warnings);
}
```

### Validation Rules

- `title` - Required, string, non-empty
- `username` - Optional, string
- `password` - Optional, string
- `url` - Optional, valid URL format
- `category` - Optional, valid EntryCategory enum value
- `tags` - Optional, array of strings
- `notes` - Optional, string

---

## validateCustomFieldData()

Validates custom field data before creation.

### Signature

```typescript
function validateCustomFieldData(data: Partial<CustomField>): ValidationResult;
```

### Parameters

| Parameter | Type                   | Description                       |
| --------- | ---------------------- | --------------------------------- |
| `data`    | `Partial<CustomField>` | The custom field data to validate |

### Returns

`ValidationResult` - Validation result with errors and warnings

### Example

```typescript
import { validateCustomFieldData, FieldType } from "safekeys-core";

const fieldData = {
  name: "API Key",
  value: "sk-1234567890",
  type: FieldType.PASSWORD,
  hidden: true,
};

const result = validateCustomFieldData(fieldData);

if (result.isValid) {
  console.log("Field data is valid");
} else {
  console.log("Validation errors:", result.errors);
}
```

### Validation Rules

- `name` - Required, string, non-empty
- `value` - Required, string
- `type` - Required, valid FieldType enum value
- `hidden` - Optional, boolean

---

## validateEntry()

Validates a complete vault entry.

### Signature

```typescript
function validateEntry(entry: VaultEntry): ValidationResult;
```

### Parameters

| Parameter | Type         | Description                  |
| --------- | ------------ | ---------------------------- |
| `entry`   | `VaultEntry` | The entry object to validate |

### Returns

`ValidationResult` - Validation result with errors and warnings

### Example

```typescript
import { validateEntry } from "safekeys-core";

const entry = {
  id: "entry_123",
  title: "Test Entry",
  username: "user@example.com",
  password: "password123",
  createdAt: new Date(),
  updatedAt: new Date(),
};

const result = validateEntry(entry);

if (result.isValid) {
  console.log("Entry is valid");
} else {
  console.log("Validation errors:", result.errors);
}
```

---

## ValidationResult Interface

The result object returned by validation functions.

### Type Definition

```typescript
interface ValidationResult {
  isValid: boolean;
  errors: ValidationError[];
  warnings: ValidationWarning[];
}

interface ValidationError {
  field: string;
  message: string;
  code: string;
}

interface ValidationWarning {
  field: string;
  message: string;
  code: string;
}
```

### Properties

| Property   | Type                  | Description                  |
| ---------- | --------------------- | ---------------------------- |
| `isValid`  | `boolean`             | Whether validation passed    |
| `errors`   | `ValidationError[]`   | Array of validation errors   |
| `warnings` | `ValidationWarning[]` | Array of validation warnings |

### Example Usage

```typescript
import { validateCreateEntryData } from "safekeys-core";

function handleEntryCreation(rawData: any) {
  const validation = validateCreateEntryData(rawData);

  if (validation.isValid) {
    const entry = createEntry(rawData);
    return { success: true, entry };
  } else {
    return {
      success: false,
      errors: validation.errors,
      warnings: validation.warnings,
    };
  }
}
```

---

## Schema Validation

SafeKeys-Core uses comprehensive schema validation to ensure data integrity.

### Entry Schema

```typescript
// Data for creating new entries
type CreateVaultEntryData = Omit<VaultEntry, "id" | "createdAt" | "updatedAt">;

// Complete entry after creation
interface VaultEntry {
  id: string; // auto-generated
  title: string; // required
  username?: string; // optional
  password?: string; // optional
  url?: string; // optional
  notes?: string; // optional
  tags?: string[]; // optional
  createdAt: Date; // auto-generated
  updatedAt: Date; // auto-generated
  favorite?: boolean; // optional
  category?: EntryCategory; // optional
  customFields?: CustomField[]; // optional
}
```

### Custom Field Schema

```typescript
interface CustomField {
  id: string; // auto-generated
  name: string; // required
  value: string; // required
  type: FieldType; // required
  hidden?: boolean; // optional, default: false
}
```

---

## Error Handling

Validation functions provide detailed error messages for debugging.

### Common Error Codes

```typescript
// Title validation
"REQUIRED_FIELD"; // Field is required
"INVALID_TYPE"; // Field has wrong type
"INVALID_FORMAT"; // Field format is invalid

// URL validation
"INVALID_URL"; // URL format is invalid

// Category validation
"INVALID_ENUM_VALUE"; // Invalid enum value

// Custom field validation
"FIELD_TOO_LONG"; // Field value too long
"FIELD_EMPTY"; // Required field is empty
```

### Example Error Handling

```typescript
import { validateCreateEntryData } from "safekeys-core";

const invalidData = {
  title: "", // Empty title
  url: "not-a-url", // Invalid URL
  category: "invalid", // Invalid category
};

const result = validateCreateEntryData(invalidData);

if (!result.isValid) {
  result.errors.forEach((error) => {
    console.error(`${error.field}: ${error.message} (${error.code})`);
  });

  // Output:
  // title: Title is required (REQUIRED_FIELD)
  // url: URL must be a valid URL format (INVALID_URL)
  // category: Invalid category value (INVALID_ENUM_VALUE)
}
```

---
