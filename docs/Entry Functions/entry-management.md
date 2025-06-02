Functions for creating, updating, and managing vault entries.

---

## createEntry()

Creates a new vault entry with the provided data.

### Signature

```typescript
function createEntry(data: CreateVaultEntryData): VaultEntry;
```

### Parameters

| Parameter | Type                   | Description                          |
| --------- | ---------------------- | ------------------------------------ |
| `data`    | `CreateVaultEntryData` | Entry data without ID and timestamps |

### Returns

`VaultEntry` - Complete vault entry with generated ID and timestamps

### Example

```typescript
import { createEntry, EntryCategory } from "safekeys-core";

const entry = createEntry({
  title: "GitHub Account",
  username: "myusername",
  password: "secure-password-123",
  url: "https://github.com",
  category: EntryCategory.LOGIN,
  tags: ["development", "work"],
  notes: "My main GitHub account",
  favorite: true,
});

console.log(entry);
// {
//   id: "entry_1234567890",
//   title: "GitHub Account",
//   username: "myusername",
//   password: "secure-password-123",
//   url: "https://github.com",
//   category: "login",
//   tags: ["development", "work"],
//   notes: "My main GitHub account",
//   favorite: true,
//   customFields: [],
//   createdAt: Date object,
//   updatedAt: Date object
// }
```

---

## updateEntry()

Updates an existing vault entry with new data.

### Signature

```typescript
function updateEntry(
  entry: VaultEntry,
  updates: UpdateVaultEntryData
): VaultEntry;
```

### Parameters

| Parameter | Type                   | Description                  |
| --------- | ---------------------- | ---------------------------- |
| `entry`   | `VaultEntry`           | The existing entry to update |
| `updates` | `UpdateVaultEntryData` | Partial data to update       |

### Returns

`VaultEntry` - Updated entry with new `updatedAt` timestamp

### Example

```typescript
import { updateEntry } from "safekeys-core";

const existingEntry = {
  /* ... existing entry ... */
};

const updatedEntry = updateEntry(existingEntry, {
  password: "new-secure-password-456",
  notes: "Updated password on 2024-01-15",
});

console.log(updatedEntry.password); // "new-secure-password-456"
console.log(updatedEntry.updatedAt); // New Date object
```

---

## validateEntry()

Validates a vault entry against the schema.

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

const entryToValidate = {
  id: "entry_123",
  title: "Test Entry",
  username: "user@example.com",
  password: "password123",
  createdAt: new Date(),
  updatedAt: new Date(),
};

const result = validateEntry(entryToValidate);

if (result.isValid) {
  console.log("Entry is valid");
} else {
  console.log("Validation errors:", result.errors);
  console.log("Validation warnings:", result.warnings);
}
```

---

## createCustomField()

Creates a custom field for vault entries.

### Signature

```typescript
function createCustomField(
  name: string,
  value: string,
  type: FieldType = FieldType.TEXT,
  hidden: boolean = false
): CustomField;
```

### Parameters

| Parameter | Type        | Description                                         |
| --------- | ----------- | --------------------------------------------------- |
| `name`    | `string`    | Display name of the field                           |
| `value`   | `string`    | Field value                                         |
| `type`    | `FieldType` | Type of the field (default: FieldType.TEXT)         |
| `hidden`  | `boolean`   | Whether the field should be hidden (default: false) |

### Returns

`CustomField` - Custom field with generated ID

### Example

```typescript
import { createCustomField, FieldType } from "safekeys-core";

const apiKeyField = createCustomField(
  "API Key",
  "sk-1234567890abcdef",
  FieldType.PASSWORD,
  true // hidden
);

console.log(apiKeyField);
// {
//   id: "field_1234567890",
//   name: "API Key",
//   value: "sk-1234567890abcdef",
//   type: "password",
//   hidden: true
// }
```

---

## updateCustomField()

Updates a custom field in an entry.

### Signature

```typescript
function updateCustomField(
  entry: VaultEntry,
  fieldId: string,
  updates: Partial<Omit<CustomField, "id">>
): VaultEntry;
```

### Parameters

| Parameter | Type                               | Description                          |
| --------- | ---------------------------------- | ------------------------------------ |
| `entry`   | `VaultEntry`                       | Entry containing the custom field    |
| `fieldId` | `string`                           | ID of the custom field to update     |
| `updates` | `Partial<Omit<CustomField, "id">>` | Updates to apply to the custom field |

### Returns

`VaultEntry` - Updated entry with modified custom field

### Example

```typescript
import { updateCustomField } from "safekeys-core";

const entryWithFields = {
  /* ... entry with custom fields ... */
};

const updatedEntry = updateCustomField(entryWithFields, "field_123", {
  value: "new-api-key-value",
  hidden: false,
});
```

---

## addCustomField()

Adds a custom field to a vault entry.

### Signature

```typescript
function addCustomField(entry: VaultEntry, field: CustomField): VaultEntry;
```

### Parameters

| Parameter | Type          | Description                   |
| --------- | ------------- | ----------------------------- |
| `entry`   | `VaultEntry`  | The entry to add the field to |
| `field`   | `CustomField` | The custom field to add       |

### Returns

`VaultEntry` - Entry with the custom field added

### Example

```typescript
import { addCustomField, createCustomField, FieldType } from "safekeys-core";

const entry = createEntry({ title: "API Service" });
const customField = createCustomField("API Key", "abc123", FieldType.PASSWORD);

const entryWithField = addCustomField(entry, customField);

console.log(entryWithField.customFields?.length); // 1
console.log(entryWithField.customFields?.[0].name); // "API Key"
```

---

## removeCustomField()

Removes a custom field from a vault entry.

### Signature

```typescript
function removeCustomField(entry: VaultEntry, fieldId: string): VaultEntry;
```

### Parameters

| Parameter | Type         | Description                        |
| --------- | ------------ | ---------------------------------- |
| `entry`   | `VaultEntry` | The entry to remove the field from |
| `fieldId` | `string`     | ID of the custom field to remove   |

### Returns

`VaultEntry` - Entry with the custom field removed

### Example

```typescript
import { removeCustomField } from "safekeys-core";

const entryWithFields = {
  /* ... entry with custom fields ... */
};
const fieldIdToRemove = "field_1234567890";

const entryWithoutField = removeCustomField(entryWithFields, fieldIdToRemove);

console.log("Fields remaining:", entryWithoutField.customFields?.length);
```

---

## validateCreateEntryData()

Validates data for creating a new vault entry.

### Signature

```typescript
function validateCreateEntryData(data: CreateVaultEntryData): ValidationResult;
```

### Parameters

| Parameter | Type                   | Description                     |
| --------- | ---------------------- | ------------------------------- |
| `data`    | `CreateVaultEntryData` | Entry creation data to validate |

### Returns

`ValidationResult` - Validation result with errors and warnings

### Example

```typescript
import { validateCreateEntryData } from "safekeys-core";

const entryData = {
  title: "New Entry",
  username: "user@example.com",
  password: "password123",
};

const result = validateCreateEntryData(entryData);

if (result.isValid) {
  const entry = createEntry(entryData);
} else {
  console.log("Validation failed:", result.errors);
}
```

---

## validateCustomFieldData()

Validates data for creating a custom field.

### Signature

```typescript
function validateCustomFieldData(data: Partial<CustomField>): ValidationResult;
```

### Parameters

| Parameter | Type                   | Description                   |
| --------- | ---------------------- | ----------------------------- |
| `data`    | `Partial<CustomField>` | Custom field data to validate |

### Returns

`ValidationResult` - Validation result with errors and warnings

### Example

```typescript
import { validateCustomFieldData, FieldType } from "safekeys-core";

const fieldData = {
  name: "Secret Key",
  value: "abc123",
  type: FieldType.PASSWORD,
};

const result = validateCustomFieldData(fieldData);

if (result.isValid) {
  const field = createCustomField(
    fieldData.name,
    fieldData.value,
    fieldData.type
  );
}
```

---
