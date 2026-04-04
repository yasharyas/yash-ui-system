# Component Extraction — Stepper Form

> Auto-extracted reusable UI components. Zero project-specific logic. Copy-paste ready.

---

=== COMPONENT START ===

## Name: TextInput

### Code:
```tsx
import React from 'react';

type Props = {
  name: string;
  label: string;
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
  error?: string;
  disabled?: boolean;
  type?: 'text' | 'number' | 'email';
  mandatory?: boolean;
  uppercase?: boolean;
  maxLength?: number;
};

export default function TextInput({
  name,
  label,
  value,
  onChange,
  placeholder,
  error,
  disabled,
  type = 'text',
  mandatory,
  uppercase,
  maxLength,
}: Props) {
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const v = e.target.value;
    onChange(uppercase ? v.toUpperCase() : v);
  };

  return (
    <div className="flex flex-col gap-1">
      <label htmlFor={name} className="text-sm font-medium text-neutral-700">
        {label}
        {mandatory && <span className="text-red-500 ml-1">*</span>}
      </label>
      <input
        id={name}
        name={name}
        type={type}
        value={value}
        onChange={handleChange}
        placeholder={placeholder}
        disabled={disabled}
        maxLength={maxLength}
        className={[
          'h-11 px-4 rounded-full border bg-white text-neutral-900 placeholder:text-neutral-400',
          'focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-all duration-200',
          uppercase ? 'uppercase' : '',
          error ? 'border-red-500 focus:border-red-500' : 'border-neutral-300',
          disabled ? 'bg-neutral-100 text-neutral-500 cursor-not-allowed' : '',
        ].join(' ')}
        autoComplete="off"
      />
      {error && <p className="text-xs text-red-500 mt-0.5">{error}</p>}
    </div>
  );
}
```

### Props:
```ts
type Props = {
  name: string;           // Input id and name attribute
  label: string;          // Visible label text
  value: string;          // Controlled value
  onChange: (value: string) => void;
  placeholder?: string;
  error?: string;         // Validation error message
  disabled?: boolean;
  type?: 'text' | 'number' | 'email';
  mandatory?: boolean;    // Shows red asterisk
  uppercase?: boolean;    // Auto-converts input to uppercase
  maxLength?: number;
};
```

### Dependencies:
- `react` (peer)
- Tailwind CSS

### Styles:
- Pill/rounded-full input shape
- `h-11` height for 44px touch target
- `focus:ring-2 focus:ring-indigo-400` focus ring
- Error state: `border-red-500`
- Disabled state: `bg-neutral-100 cursor-not-allowed`

### Usage:
```tsx
<TextInput
  name="email"
  label="Email Address"
  value={email}
  onChange={setEmail}
  placeholder="you@example.com"
  type="email"
  mandatory
  error={emailError}
/>
```

### Prompt:
> Create a reusable React TextInput component with Tailwind CSS. It should support text, email, and number types. Display a label above the input, show a red asterisk when mandatory, support an error message below, have a pill/capsule shape (rounded-full), uppercase transform option, and disabled state styling.

### Registry JSON:
```json
{
  "name": "TextInput",
  "category": "forms",
  "tags": ["input", "text", "pill", "accessible", "controlled", "validation"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

## Name: RadioGroup

### Code:
```tsx
import React from 'react';

type Props = {
  name: string;
  label: string;
  value: string;
  onChange: (value: string) => void;
  options: string[];
  error?: string;
  mandatory?: boolean;
};

export default function RadioGroup({
  name,
  label,
  value,
  onChange,
  options,
  error,
  mandatory,
}: Props) {
  return (
    <fieldset className="flex flex-col gap-2">
      <legend className="text-sm font-medium text-neutral-700 mb-1">
        {label}
        {mandatory && <span className="text-red-500 ml-1">*</span>}
      </legend>
      <div className="flex flex-wrap gap-3">
        {options.map(option => (
          <label
            key={option}
            className={[
              'flex items-center gap-2 px-4 py-2.5 rounded-full border cursor-pointer',
              'transition-all duration-200 min-h-[48px] select-none',
              value === option
                ? 'border-indigo-500 bg-indigo-50 text-indigo-600 font-medium'
                : 'border-neutral-300 bg-white text-neutral-700 hover:border-neutral-400',
            ].join(' ')}
          >
            <input
              type="radio"
              name={name}
              value={option}
              checked={value === option}
              onChange={() => onChange(option)}
              className="sr-only"
            />
            <span
              className={[
                'w-4 h-4 rounded-full border-2 flex items-center justify-center flex-shrink-0',
                value === option ? 'border-indigo-500' : 'border-neutral-400',
              ].join(' ')}
            >
              {value === option && (
                <span className="w-2 h-2 rounded-full bg-indigo-500" />
              )}
            </span>
            <span className="text-sm">{option}</span>
          </label>
        ))}
      </div>
      {error && <p className="text-xs text-red-500 mt-0.5">{error}</p>}
    </fieldset>
  );
}
```

### Props:
```ts
type Props = {
  name: string;           // Radio group name attribute
  label: string;          // Fieldset legend text
  value: string;          // Currently selected option
  onChange: (value: string) => void;
  options: string[];      // Array of option strings
  error?: string;
  mandatory?: boolean;
};
```

### Dependencies:
- `react` (peer)
- Tailwind CSS

### Styles:
- Pill-shaped option chips (rounded-full)
- Selected state: `bg-indigo-50 border-indigo-500 text-indigo-600`
- Custom radio dot rendered via spans (native input hidden with `sr-only`)
- Min 48px height for touch targets

### Usage:
```tsx
<RadioGroup
  name="gender"
  label="Gender"
  value={gender}
  onChange={setGender}
  options={['Male', 'Female', 'Other']}
  mandatory
  error={genderError}
/>
```

### Prompt:
> Create a RadioGroup component using Tailwind CSS. Each option renders as a pill-shaped chip. The selected chip has a filled indigo background tint and custom dot indicator. The native radio input is visually hidden. Supports error message and mandatory asterisk.

### Registry JSON:
```json
{
  "name": "RadioGroup",
  "category": "forms",
  "tags": ["radio", "chip", "pill", "selection", "accessible", "fieldset"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

## Name: Checkbox

### Code:
```tsx
import React from 'react';

type Props = {
  name: string;
  label: string;
  checked: boolean;
  onChange: (checked: boolean) => void;
  error?: string;
  mandatory?: boolean;
};

export default function Checkbox({
  name,
  label,
  checked,
  onChange,
  error,
  mandatory,
}: Props) {
  return (
    <div className="flex flex-col gap-1">
      <label
        htmlFor={name}
        className="flex items-center gap-3 cursor-pointer min-h-[48px] select-none"
      >
        <div
          className={[
            'w-5 h-5 rounded-full border-2 flex items-center justify-center flex-shrink-0 transition-all duration-200',
            checked ? 'bg-indigo-500 border-indigo-500' : 'border-neutral-400 bg-white',
            error ? 'border-red-500' : '',
          ].join(' ')}
        >
          {checked && (
            <svg className="w-3 h-3 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={3}>
              <path strokeLinecap="round" strokeLinejoin="round" d="M5 13l4 4L19 7" />
            </svg>
          )}
        </div>
        <input
          id={name}
          name={name}
          type="checkbox"
          checked={checked}
          onChange={e => onChange(e.target.checked)}
          className="sr-only"
        />
        <span className="text-sm text-neutral-700">
          {label}
          {mandatory && <span className="text-red-500 ml-1">*</span>}
        </span>
      </label>
      {error && <p className="text-xs text-red-500 mt-0.5">{error}</p>}
    </div>
  );
}
```

### Props:
```ts
type Props = {
  name: string;
  label: string;
  checked: boolean;
  onChange: (checked: boolean) => void;
  error?: string;
  mandatory?: boolean;
};
```

### Dependencies:
- `react` (peer)
- Tailwind CSS

### Styles:
- Custom circular checkbox (rounded-full) — distinct from standard square
- Checked state: `bg-indigo-500` with inline SVG checkmark
- Native input hidden with `sr-only`
- Error highlights border red

### Usage:
```tsx
<Checkbox
  name="terms"
  label="I agree to the Terms and Conditions"
  checked={agreed}
  onChange={setAgreed}
  mandatory
  error={termsError}
/>
```

### Prompt:
> Create a custom Checkbox component with Tailwind CSS. Use a circular (rounded-full) indicator instead of a square. When checked, fill it with indigo and show a white SVG checkmark. The native checkbox input is sr-only. Show an optional error message below.

### Registry JSON:
```json
{
  "name": "Checkbox",
  "category": "forms",
  "tags": ["checkbox", "toggle", "circular", "accessible", "custom"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

## Name: DOBPicker

### Code:
```tsx
import React, { useState, useEffect } from 'react';

type Props = {
  name: string;
  label: string;
  value: string;       // ISO format: yyyy-mm-dd (empty string when unset)
  onChange: (value: string) => void;
  error?: string;
  mandatory?: boolean;
};

function toDisplay(iso: string): string {
  if (!iso) return '';
  const parts = iso.split('-');
  if (parts.length !== 3) return '';
  const [y, m, d] = parts;
  return `${d}/${m}/${y}`;
}

function toISO(display: string): string {
  const digits = display.replace(/\D/g, '');
  if (digits.length !== 8) return '';
  const d = digits.slice(0, 2);
  const m = digits.slice(2, 4);
  const y = digits.slice(4, 8);
  const date = new Date(parseInt(y), parseInt(m) - 1, parseInt(d));
  if (
    date.getFullYear() !== parseInt(y) ||
    date.getMonth() !== parseInt(m) - 1 ||
    date.getDate() !== parseInt(d)
  ) return '';
  return `${y}-${m.padStart(2, '0')}-${d.padStart(2, '0')}`;
}

export default function DOBPicker({ name, label, value, onChange, error, mandatory }: Props) {
  const [displayValue, setDisplayValue] = useState(() => toDisplay(value));

  useEffect(() => {
    setDisplayValue(toDisplay(value));
  }, [value]);

  const handleChange = (raw: string) => {
    const incoming = raw.replace(/\D/g, '').slice(0, 8);
    const prev = displayValue.replace(/\D/g, '');

    if (incoming.length <= prev.length) {
      let formatted = incoming;
      if (incoming.length > 4) formatted = `${incoming.slice(0, 2)}/${incoming.slice(2, 4)}/${incoming.slice(4)}`;
      else if (incoming.length > 2) formatted = `${incoming.slice(0, 2)}/${incoming.slice(2)}`;
      setDisplayValue(formatted);
      onChange(incoming.length === 8 ? (toISO(formatted) || '') : '');
      return;
    }

    let sanitized = '';
    for (let i = 0; i < incoming.length; i++) {
      const d = parseInt(incoming[i], 10);
      if (i === 0) { if (d > 3) return; sanitized += incoming[i]; }
      else if (i === 1) { const day = parseInt(incoming.slice(0, 2), 10); if (day < 1 || day > 31) return; sanitized += incoming[i]; }
      else if (i === 2) { if (d > 1) return; sanitized += incoming[i]; }
      else if (i === 3) { const month = parseInt(incoming.slice(2, 4), 10); if (month < 1 || month > 12) return; sanitized += incoming[i]; }
      else sanitized += incoming[i];
    }

    let formatted = sanitized;
    if (sanitized.length > 4) formatted = `${sanitized.slice(0, 2)}/${sanitized.slice(2, 4)}/${sanitized.slice(4)}`;
    else if (sanitized.length > 2) formatted = `${sanitized.slice(0, 2)}/${sanitized.slice(2)}`;
    setDisplayValue(formatted);
    onChange(sanitized.length === 8 ? (toISO(formatted) || '') : '');
  };

  return (
    <div className="flex flex-col gap-1">
      <label htmlFor={name} className="text-sm font-medium text-neutral-700">
        {label}
        {mandatory && <span className="text-red-500 ml-1">*</span>}
      </label>
      <input
        id={name}
        name={name}
        type="text"
        inputMode="numeric"
        placeholder="DD/MM/YYYY"
        value={displayValue}
        onChange={e => handleChange(e.target.value)}
        maxLength={10}
        className={[
          'h-11 px-4 rounded-full border bg-white text-neutral-900 placeholder:text-neutral-400',
          'focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-all duration-200',
          error ? 'border-red-500' : 'border-neutral-300',
        ].join(' ')}
      />
      {error && <p className="text-xs text-red-500 mt-0.5">{error}</p>}
    </div>
  );
}
```

### Props:
```ts
type Props = {
  name: string;
  label: string;
  value: string;       // ISO yyyy-mm-dd or empty string
  onChange: (value: string) => void;  // emits ISO yyyy-mm-dd or ''
  error?: string;
  mandatory?: boolean;
};
```

### Dependencies:
- `react` (peer)
- Tailwind CSS

### Styles:
- Same pill input style as TextInput
- `inputMode="numeric"` for mobile numeric keyboard

### Usage:
```tsx
<DOBPicker
  name="dob"
  label="Date of Birth"
  value={dob}           // "1990-05-20" or ""
  onChange={setDob}     // receives "1990-05-20" or ""
  mandatory
  error={dobError}
/>
```

### Prompt:
> Create a Date of Birth picker using a plain text input in React + Tailwind. Display in DD/MM/YYYY format with auto-inserted slashes as the user types. Validate day (01–31) and month (01–12) per digit. Internally convert to/from ISO yyyy-mm-dd. Show numeric keyboard on mobile. Support error and mandatory props.

### Registry JSON:
```json
{
  "name": "DOBPicker",
  "category": "forms",
  "tags": ["date", "dob", "masked-input", "dd-mm-yyyy", "accessible", "numeric"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

## Name: SelectInput

### Code:
```tsx
import React from 'react';
import * as Select from '@radix-ui/react-select';
import { ChevronDown, Check } from 'lucide-react';

type Props = {
  name: string;
  label: string;
  value: string;
  onChange: (value: string) => void;
  options: string[];
  placeholder?: string;
  error?: string;
  mandatory?: boolean;
};

export default function SelectInput({
  name,
  label,
  value,
  onChange,
  options,
  placeholder,
  error,
  mandatory,
}: Props) {
  return (
    <div className="flex flex-col gap-1">
      <label htmlFor={name} className="text-sm font-medium text-neutral-700">
        {label}
        {mandatory && <span className="text-red-500 ml-1">*</span>}
      </label>
      <Select.Root value={value || undefined} onValueChange={onChange}>
        <Select.Trigger
          id={name}
          className={[
            'h-11 px-4 rounded-full border bg-white flex items-center justify-between gap-2 text-left',
            'focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-all duration-200',
            !value ? 'text-neutral-400' : 'text-neutral-900',
            error ? 'border-red-500' : 'border-neutral-300',
            'data-[state=open]:border-indigo-500 data-[state=open]:ring-2 data-[state=open]:ring-indigo-300',
          ].join(' ')}
        >
          <Select.Value placeholder={placeholder ?? 'Select an option'} />
          <Select.Icon>
            <ChevronDown className="w-4 h-4 text-neutral-400" />
          </Select.Icon>
        </Select.Trigger>

        <Select.Portal>
          <Select.Content
            className="bg-white border border-neutral-200 rounded-lg shadow-md overflow-hidden z-50 min-w-[var(--radix-select-trigger-width)]"
            position="popper"
            sideOffset={4}
          >
            <Select.Viewport className="p-1">
              {options.map(option => (
                <Select.Item
                  key={option}
                  value={option}
                  className={[
                    'flex items-center gap-2 px-3 py-2.5 text-sm rounded-md cursor-pointer outline-none text-neutral-700',
                    'data-[highlighted]:bg-indigo-50 data-[highlighted]:text-indigo-600',
                    'data-[state=checked]:font-medium data-[state=checked]:text-indigo-600',
                    'transition-colors duration-150',
                  ].join(' ')}
                >
                  <Select.ItemText>{option}</Select.ItemText>
                  <Select.ItemIndicator className="ml-auto">
                    <Check className="w-4 h-4" />
                  </Select.ItemIndicator>
                </Select.Item>
              ))}
            </Select.Viewport>
          </Select.Content>
        </Select.Portal>
      </Select.Root>
      {error && <p className="text-xs text-red-500 mt-0.5">{error}</p>}
    </div>
  );
}
```

### Props:
```ts
type Props = {
  name: string;
  label: string;
  value: string;
  onChange: (value: string) => void;
  options: string[];
  placeholder?: string;
  error?: string;
  mandatory?: boolean;
};
```

### Dependencies:
- `react` (peer)
- `@radix-ui/react-select` — `npm install @radix-ui/react-select`
- `lucide-react` — `npm install lucide-react`
- Tailwind CSS

### Styles:
- Pill trigger (rounded-full)
- Dropdown: `rounded-lg`, `shadow-md`, `bg-white`
- Highlighted item: `bg-indigo-50 text-indigo-600`
- Checked item shows `<Check />` icon from lucide-react

### Usage:
```tsx
<SelectInput
  name="state"
  label="State"
  value={selectedState}
  onChange={setSelectedState}
  options={['Maharashtra', 'Karnataka', 'Delhi']}
  placeholder="Choose a state"
  mandatory
  error={stateError}
/>
```

### Prompt:
> Build a SelectInput component with Radix UI Select and Tailwind CSS. The trigger should be pill-shaped (rounded-full, h-11). The dropdown list renders inside a Radix Portal with keyboard navigation. Show a chevron icon on the trigger and a check icon on the selected item. Support label, placeholder, error, and mandatory props.

### Registry JSON:
```json
{
  "name": "SelectInput",
  "category": "forms",
  "tags": ["select", "dropdown", "radix-ui", "accessible", "pill", "portal"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

## Name: FileUpload

### Code:
```tsx
import React, { useRef, useState } from 'react';
import { Upload, Eye, X, FileText } from 'lucide-react';

type Props = {
  name: string;
  label: string;
  value: string;           // filename string (empty = no file)
  onChange: (value: string) => void;
  error?: string;
  mandatory?: boolean;
  accept?: string;         // e.g. ".pdf,.jpg,.png"
};

export default function FileUpload({
  name,
  label,
  value,
  onChange,
  error,
  mandatory,
  accept = '.pdf,.jpg,.jpeg,.png,.doc,.docx',
}: Props) {
  const inputRef = useRef<HTMLInputElement>(null);
  const [previewUrl, setPreviewUrl] = useState<string | null>(null);
  const [showPreview, setShowPreview] = useState(false);

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (file) {
      onChange(file.name);
      if (file.type.startsWith('image/')) {
        const url = URL.createObjectURL(file);
        setPreviewUrl(url);
      } else {
        setPreviewUrl(null);
      }
    }
  };

  const handleRemove = () => {
    onChange('');
    if (previewUrl) {
      URL.revokeObjectURL(previewUrl);
      setPreviewUrl(null);
    }
    setShowPreview(false);
    if (inputRef.current) inputRef.current.value = '';
  };

  return (
    <div className="flex flex-col gap-1">
      <label className="text-sm font-medium text-neutral-700">
        {label}
        {mandatory && <span className="text-red-500 ml-1">*</span>}
      </label>

      {!value ? (
        <div
          className={[
            'flex items-center gap-3 p-3 rounded-xl border border-dashed cursor-pointer',
            'transition-all duration-200 min-h-[48px]',
            error ? 'border-red-500' : 'border-neutral-300 hover:border-indigo-400',
          ].join(' ')}
          onClick={() => inputRef.current?.click()}
          role="button"
          tabIndex={0}
          onKeyDown={e => { if (e.key === 'Enter' || e.key === ' ') inputRef.current?.click(); }}
        >
          <Upload className="w-5 h-5 text-neutral-400 flex-shrink-0" />
          <span className="text-sm text-neutral-500">Click to upload a file</span>
        </div>
      ) : (
        <div className="flex items-center gap-2 p-3 rounded-xl border border-neutral-300 bg-neutral-50">
          <FileText className="w-5 h-5 text-indigo-500 flex-shrink-0" />
          <span className="text-sm text-neutral-700 truncate flex-1">{value}</span>
          <div className="flex items-center gap-1">
            {previewUrl && (
              <button
                type="button"
                onClick={() => setShowPreview(true)}
                className="p-1.5 rounded-full hover:bg-neutral-200 transition-colors"
                title="Preview"
              >
                <Eye className="w-4 h-4 text-neutral-600" />
              </button>
            )}
            <button
              type="button"
              onClick={handleRemove}
              className="p-1.5 rounded-full hover:bg-neutral-200 transition-colors"
              title="Remove"
            >
              <X className="w-4 h-4 text-neutral-600" />
            </button>
          </div>
        </div>
      )}

      <input
        ref={inputRef}
        id={name}
        name={name}
        type="file"
        onChange={handleChange}
        className="sr-only"
        accept={accept}
      />

      {error && <p className="text-xs text-red-500 mt-0.5">{error}</p>}

      {showPreview && previewUrl && (
        <div
          className="fixed inset-0 z-50 flex items-center justify-center bg-black/40 backdrop-blur-sm"
          onClick={() => setShowPreview(false)}
        >
          <div
            className="bg-white rounded-xl shadow-lg p-4 max-w-lg w-full mx-4"
            onClick={e => e.stopPropagation()}
          >
            <div className="flex items-center justify-between mb-3">
              <span className="text-sm font-semibold text-neutral-800">{value}</span>
              <button
                type="button"
                onClick={() => setShowPreview(false)}
                className="p-1.5 rounded-full hover:bg-neutral-200 transition-colors"
              >
                <X className="w-4 h-4 text-neutral-600" />
              </button>
            </div>
            <img src={previewUrl} alt={value} className="w-full rounded-lg object-contain max-h-[60vh]" />
          </div>
        </div>
      )}
    </div>
  );
}
```

### Props:
```ts
type Props = {
  name: string;
  label: string;
  value: string;           // filename — empty string means no file selected
  onChange: (value: string) => void;
  error?: string;
  mandatory?: boolean;
  accept?: string;         // defaults to ".pdf,.jpg,.jpeg,.png,.doc,.docx"
};
```

### Dependencies:
- `react` (peer)
- `lucide-react` — `npm install lucide-react`
- Tailwind CSS (with `backdrop-blur-sm` enabled)

### Styles:
- Upload zone: `border-dashed rounded-xl` with hover highlight
- File selected: `bg-neutral-50 rounded-xl` with filename and action buttons
- Image preview: full-screen modal with `backdrop-blur-sm` overlay

### Usage:
```tsx
<FileUpload
  name="passport"
  label="Upload Passport Copy"
  value={fileName}
  onChange={setFileName}
  mandatory
  error={fileError}
  accept=".pdf,.jpg,.jpeg,.png"
/>
```

### Prompt:
> Create a FileUpload component with Tailwind CSS and lucide-react icons. In empty state show a dashed border drop zone with an Upload icon. Once a file is selected, show the filename with Remove (X) and Preview (Eye) buttons. For image files, generate an object URL and show a full-screen preview modal with backdrop blur. Clean up object URLs on remove.

### Registry JSON:
```json
{
  "name": "FileUpload",
  "category": "forms",
  "tags": ["file", "upload", "preview", "modal", "drag-drop", "image"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

## Name: Stepper

### Code:
```tsx
import React from 'react';

type Step = {
  id: string;
  title: string;
};

type Props = {
  steps: Step[];
  currentStepIndex: number;        // 0-based
  completedStepIds: Set<string>;   // set of step.id strings
};

export default function Stepper({ steps, currentStepIndex, completedStepIds }: Props) {
  const totalSteps = steps.length;
  const currentStep = currentStepIndex + 1;

  return (
    <div className="w-full">
      {/* Mobile: progress bar */}
      <div className="sm:hidden">
        <div className="flex items-center justify-between mb-2">
          <span className="text-xs font-medium text-neutral-500">
            Step {currentStep} of {totalSteps}
          </span>
          <span className="text-xs font-semibold text-indigo-600">
            {steps[currentStepIndex]?.title}
          </span>
        </div>
        <div className="w-full h-2 bg-neutral-200 rounded-full overflow-hidden">
          <div
            className="h-full bg-indigo-500 rounded-full transition-all duration-500 ease-out"
            style={{ width: `${(currentStep / totalSteps) * 100}%` }}
          />
        </div>
        <div className="flex justify-between mt-1.5">
          {steps.map((step, i) => {
            const isCompleted = completedStepIds.has(step.id);
            const isCurrent = i === currentStepIndex;
            return (
              <div
                key={step.id}
                className={[
                  'w-2 h-2 rounded-full transition-all duration-300',
                  isCompleted ? 'bg-green-500' : isCurrent ? 'bg-indigo-500 scale-125' : 'bg-neutral-300',
                ].join(' ')}
              />
            );
          })}
        </div>
      </div>

      {/* Desktop: labelled step indicators */}
      <div className="hidden sm:block w-full pb-2">
        <div className="flex items-start w-full px-4">
          {steps.map((step, index) => {
            const isCompleted = completedStepIds.has(step.id);
            const isCurrent = index === currentStepIndex;

            return (
              <React.Fragment key={step.id}>
                <div className="flex flex-col items-center flex-shrink-0 w-16">
                  <div
                    className={[
                      'w-9 h-9 rounded-full flex items-center justify-center text-sm font-semibold transition-all duration-300',
                      isCompleted
                        ? 'bg-green-500 text-white'
                        : isCurrent
                          ? 'bg-indigo-500 text-white shadow-[0_0_0_3px_rgba(99,102,241,0.3)]'
                          : 'bg-neutral-200 text-neutral-500',
                    ].join(' ')}
                  >
                    {isCompleted ? (
                      <svg className="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={3}>
                        <path strokeLinecap="round" strokeLinejoin="round" d="M5 13l4 4L19 7" />
                      </svg>
                    ) : (
                      index + 1
                    )}
                  </div>
                  <span
                    className={[
                      'text-xs text-center w-full leading-tight mt-1.5',
                      isCurrent ? 'text-indigo-600 font-semibold' : isCompleted ? 'text-green-600 font-medium' : 'text-neutral-400',
                    ].join(' ')}
                  >
                    {step.title}
                  </span>
                </div>
                {index < steps.length - 1 && (
                  <div
                    className={[
                      'flex-1 h-0.5 mt-[18px] transition-all duration-300',
                      isCompleted ? 'bg-green-500' : 'bg-neutral-200',
                    ].join(' ')}
                  />
                )}
              </React.Fragment>
            );
          })}
        </div>
      </div>
    </div>
  );
}
```

### Props:
```ts
type Step = {
  id: string;
  title: string;
};

type Props = {
  steps: Step[];
  currentStepIndex: number;        // 0-based index of active step
  completedStepIds: Set<string>;   // IDs of completed steps
};
```

### Dependencies:
- `react` (peer)
- Tailwind CSS

### Styles:
- **Mobile**: pill progress bar + dot indicators per step
- **Desktop**: circular step bubbles connected by horizontal lines
- Completed: `bg-green-500` with SVG checkmark
- Current: `bg-indigo-500` with focus ring shadow
- Connector line turns green when step completed

### Usage:
```tsx
const steps = [
  { id: 'personal', title: 'Personal' },
  { id: 'address', title: 'Address' },
  { id: 'review', title: 'Review' },
];

<Stepper
  steps={steps}
  currentStepIndex={1}
  completedStepIds={new Set(['personal'])}
/>
```

### Prompt:
> Build a responsive Stepper/progress indicator component in React + Tailwind. On mobile, show a filled progress bar with step count text and small dot indicators. On desktop (sm+), show circular numbered bubbles connected by horizontal lines. Completed steps show a green checkmark, the current step is highlighted in indigo with a glow ring, and future steps are grey. Accept steps array, currentStepIndex, and completedStepIds as props.

### Registry JSON:
```json
{
  "name": "Stepper",
  "category": "navigation",
  "tags": ["stepper", "progress", "wizard", "multi-step", "responsive", "accessible"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

## Name: StepperNavigation

### Code:
```tsx
import React from 'react';
import { ChevronLeft, ChevronRight, Loader2 } from 'lucide-react';

type Props = {
  currentStepIndex: number;
  totalSteps: number;
  isLoading?: boolean;
  loadingLabel?: string;       // label shown on next/submit while loading
  isLastStep?: boolean;        // if true, shows Submit button
  onPrevious: () => void;
  onNext: () => void;
  onSubmit: () => void;
  submitLabel?: string;        // defaults to "Submit"
  nextLabel?: string;          // defaults to "Next"
  previousLabel?: string;      // defaults to "Previous"
};

export default function StepperNavigation({
  currentStepIndex,
  totalSteps,
  isLoading = false,
  loadingLabel,
  isLastStep = false,
  onPrevious,
  onNext,
  onSubmit,
  submitLabel = 'Submit',
  nextLabel = 'Next',
  previousLabel = 'Previous',
}: Props) {
  const isFirstStep = currentStepIndex === 0;

  return (
    <div className="flex items-center gap-4 pt-6 mt-6 border-t border-neutral-200 w-full">
      {!isFirstStep && (
        <button
          type="button"
          onClick={onPrevious}
          disabled={isLoading}
          className="flex-1 h-9 px-5 rounded-full font-semibold bg-neutral-100 text-neutral-800
                     hover:bg-neutral-200 focus:outline-none focus:ring-2 focus:ring-indigo-300
                     transition-all duration-200 disabled:opacity-50
                     inline-flex items-center justify-center gap-2"
        >
          <ChevronLeft className="w-4 h-4" />
          {previousLabel}
        </button>
      )}

      {isLastStep ? (
        <button
          type="button"
          onClick={onSubmit}
          disabled={isLoading}
          className="flex-1 h-9 px-5 rounded-full font-semibold bg-indigo-500 text-white
                     hover:bg-indigo-600 active:bg-indigo-700
                     focus:outline-none focus:ring-2 focus:ring-indigo-300
                     transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed
                     inline-flex items-center justify-center gap-2"
        >
          {isLoading ? (
            <>
              <Loader2 className="w-4 h-4 animate-spin" />
              {loadingLabel ?? 'Submitting...'}
            </>
          ) : (
            <>
              {submitLabel}
              <ChevronRight className="w-4 h-4" />
            </>
          )}
        </button>
      ) : (
        <button
          type="button"
          onClick={onNext}
          disabled={isLoading}
          className="flex-1 h-9 px-5 rounded-full font-semibold bg-indigo-500 text-white
                     hover:bg-indigo-600 active:bg-indigo-700
                     focus:outline-none focus:ring-2 focus:ring-indigo-300
                     transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed
                     inline-flex items-center justify-center gap-2"
        >
          {isLoading ? (
            <>
              <Loader2 className="w-4 h-4 animate-spin" />
              {loadingLabel ?? 'Saving...'}
            </>
          ) : (
            <>
              {nextLabel}
              <ChevronRight className="w-4 h-4" />
            </>
          )}
        </button>
      )}
    </div>
  );
}
```

### Props:
```ts
type Props = {
  currentStepIndex: number;
  totalSteps: number;
  isLoading?: boolean;
  loadingLabel?: string;
  isLastStep?: boolean;
  onPrevious: () => void;
  onNext: () => void;
  onSubmit: () => void;
  submitLabel?: string;
  nextLabel?: string;
  previousLabel?: string;
};
```

### Dependencies:
- `react` (peer)
- `lucide-react` — `npm install lucide-react`
- Tailwind CSS

### Styles:
- Both buttons: `h-9 rounded-full` pill shape, `flex-1` to fill row equally
- Previous: secondary (`bg-neutral-100`)
- Next/Submit: primary (`bg-indigo-500 text-white`)
- Loading state: `Loader2` spinner with `animate-spin`
- Separated from content by `border-t border-neutral-200`

### Usage:
```tsx
<StepperNavigation
  currentStepIndex={step}
  totalSteps={4}
  isLastStep={step === 3}
  isLoading={isSaving}
  onPrevious={() => setStep(s => s - 1)}
  onNext={() => setStep(s => s + 1)}
  onSubmit={handleSubmit}
/>
```

### Prompt:
> Create a StepperNavigation component for a multi-step form using React + Tailwind + lucide-react. Show a Previous button (secondary style) on all steps except the first. Show a Next button (primary indigo) or a Submit button on the last step. While loading, replace button text with a spinning Loader2 icon and disabled state. Both buttons are pill-shaped and share equal width via flex-1.

### Registry JSON:
```json
{
  "name": "StepperNavigation",
  "category": "navigation",
  "tags": ["stepper", "navigation", "previous", "next", "submit", "loading", "wizard"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

## Name: SubmissionLoader

### Code:
```tsx
import React from 'react';

type Phase = 'verifying' | 'validating' | 'submitting' | 'complete';

type PhaseConfig = {
  text: string;
  subtitle: string;
};

const DEFAULT_PHASES: Phase[] = ['verifying', 'validating', 'submitting', 'complete'];

const DEFAULT_CONFIG: Record<Phase, PhaseConfig> = {
  verifying:  { text: 'Verifying Details',     subtitle: 'Checking your information...' },
  validating: { text: 'Validating Documents',  subtitle: 'Reviewing uploaded files...' },
  submitting: { text: 'Submitting',            subtitle: 'Saving your submission...' },
  complete:   { text: 'All Done!',             subtitle: 'Your submission was successful.' },
};

type Props = {
  phase: Phase | null;                    // null = hidden
  phases?: Phase[];                       // custom phase order
  phaseConfig?: Record<Phase, PhaseConfig>;
};

export default function SubmissionLoader({ phase, phases = DEFAULT_PHASES, phaseConfig = DEFAULT_CONFIG }: Props) {
  if (!phase) return null;

  const config = phaseConfig[phase];
  const isComplete = phase === 'complete';
  const currentIndex = phases.indexOf(phase);

  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/20 backdrop-blur-sm">
      <div className="bg-white rounded-2xl shadow-lg p-8 sm:p-12 max-w-sm w-full mx-4 text-center">

        {/* Icon */}
        <div className="flex justify-center mb-6">
          {isComplete ? (
            <div className="w-16 h-16 sm:w-20 sm:h-20 rounded-full bg-green-100 flex items-center justify-center">
              <svg className="w-8 h-8 sm:w-10 sm:h-10 text-green-500" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2.5}>
                <path strokeLinecap="round" strokeLinejoin="round" d="M5 13l4 4L19 7" />
              </svg>
            </div>
          ) : (
            <div className="w-16 h-16 rounded-full bg-indigo-50 flex items-center justify-center">
              <svg className="w-8 h-8 text-indigo-500 animate-spin" fill="none" viewBox="0 0 24 24">
                <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4" />
                <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z" />
              </svg>
            </div>
          )}
        </div>

        {/* Text */}
        <h3 className="text-lg sm:text-xl font-semibold text-neutral-900 mb-2">{config.text}</h3>
        <p className="text-sm text-neutral-500 mb-6">{config.subtitle}</p>

        {/* Progress dots */}
        <div className="flex items-center justify-center gap-2">
          {phases.map((p, i) => (
            <div
              key={p}
              className={[
                'w-2.5 h-2.5 rounded-full transition-all duration-300',
                i <= currentIndex
                  ? isComplete ? 'bg-green-500' : 'bg-indigo-500'
                  : 'bg-neutral-200',
              ].join(' ')}
            />
          ))}
        </div>
        <p className="text-xs text-neutral-400 mt-3">
          Step {currentIndex + 1} of {phases.length}
        </p>
      </div>
    </div>
  );
}
```

### Props:
```ts
type Phase = 'verifying' | 'validating' | 'submitting' | 'complete';

type Props = {
  phase: Phase | null;                       // null hides the overlay
  phases?: Phase[];                          // custom order, defaults to all 4
  phaseConfig?: Record<Phase, PhaseConfig>;  // custom labels/subtitles
};
```

### Dependencies:
- `react` (peer)
- Tailwind CSS (with `backdrop-blur-sm` enabled)

### Styles:
- Fixed full-screen overlay: `bg-black/20 backdrop-blur-sm`
- Card: `bg-white rounded-2xl shadow-lg`
- Spinner: inline SVG with `animate-spin`
- Complete icon: green circle with SVG checkmark
- Progress dots turn green on complete, indigo during loading

### Usage:
```tsx
// Show during submission
<SubmissionLoader phase={submissionPhase} />

// phase = null        → hidden
// phase = 'verifying' → "Verifying Details" screen
// phase = 'complete'  → success screen with green checkmark
```

### Prompt:
> Create a full-screen submission loading overlay component in React + Tailwind. It shows a backdrop-blurred dark overlay with a centered white card. Cycle through named phases (verifying, validating, submitting, complete) with heading text and subtitle per phase. Show a spinning SVG loader for non-complete phases and a green checkmark circle for complete. Show dot progress indicators at the bottom. Accept phase as a prop; render null when phase is null.

### Registry JSON:
```json
{
  "name": "SubmissionLoader",
  "category": "feedback",
  "tags": ["loader", "overlay", "spinner", "success", "modal", "multi-phase", "progress"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

## Name: ScreenLayout

### Code:
```tsx
import React from 'react';

type Props = {
  brandInitials?: string;      // e.g. "YB"
  brandName?: string;          // e.g. "Your Brand Name"
  title: string;
  subtitle?: string;
  stepper?: React.ReactNode;   // pass <Stepper /> or null
  navigation?: React.ReactNode; // pass <StepperNavigation /> or null
  children: React.ReactNode;
};

export default function ScreenLayout({
  brandInitials = 'YB',
  brandName = 'Your Brand Name',
  title,
  subtitle,
  stepper,
  navigation,
  children,
}: Props) {
  return (
    <div className="min-h-screen bg-[#F7F8FA] flex flex-col">
      {/* Header */}
      <header className="bg-white border-b border-neutral-200 px-4 py-4 sm:px-6">
        <div className="max-w-3xl mx-auto">
          <div className="flex items-center gap-3 mb-1">
            <div className="w-8 h-8 rounded-full bg-indigo-500 flex items-center justify-center text-white font-bold text-xs">
              {brandInitials}
            </div>
            <span className="text-base font-semibold text-neutral-800">{brandName}</span>
          </div>
        </div>
      </header>

      <div className="flex-1 px-4 py-6 sm:px-6">
        <div className="max-w-3xl mx-auto">
          {stepper && <div className="mb-6">{stepper}</div>}

          <div className="bg-white rounded-xl shadow-[0_4px_24px_-4px_rgba(0,0,0,0.12),0_1px_4px_-1px_rgba(0,0,0,0.06)] border border-neutral-200/80 p-5 sm:p-8">
            <div className="mb-6">
              <h2 className="text-xl font-semibold text-neutral-900 sm:text-2xl">{title}</h2>
              {subtitle && <p className="text-sm text-neutral-500 mt-1">{subtitle}</p>}
            </div>

            {children}

            {navigation && <div>{navigation}</div>}
          </div>
        </div>
      </div>
    </div>
  );
}
```

### Props:
```ts
type Props = {
  brandInitials?: string;
  brandName?: string;
  title: string;
  subtitle?: string;
  stepper?: React.ReactNode;
  navigation?: React.ReactNode;
  children: React.ReactNode;
};
```

### Dependencies:
- `react` (peer)
- Tailwind CSS

### Styles:
- App background: `bg-[#F7F8FA]`
- Header: `bg-white border-b`
- Card: `rounded-xl` with a layered box-shadow for depth
- Constrained to `max-w-3xl mx-auto`

### Usage:
```tsx
<ScreenLayout
  brandInitials="AC"
  brandName="Acme Corp"
  title="Personal Details"
  subtitle="Fill in your basic information"
  stepper={<Stepper steps={steps} currentStepIndex={0} completedStepIds={new Set()} />}
  navigation={
    <StepperNavigation
      currentStepIndex={0}
      totalSteps={3}
      isLastStep={false}
      onPrevious={() => {}}
      onNext={handleNext}
      onSubmit={handleSubmit}
    />
  }
>
  <TextInput name="name" label="Full Name" value={name} onChange={setName} mandatory />
</ScreenLayout>
```

### Prompt:
> Create a ScreenLayout wrapper component for a multi-step form in React + Tailwind. Include a top header bar with a brand avatar (circle with initials) and brand name. The main content area is centred and max-width constrained. Render an optional stepper above a white card. Inside the card show a title, optional subtitle, slotted children, and an optional navigation slot at the bottom. Subtle layered box-shadow on the card.

### Registry JSON:
```json
{
  "name": "ScreenLayout",
  "category": "layout",
  "tags": ["layout", "shell", "header", "card", "wizard", "container"]
}
```

=== COMPONENT END ===

---

## Shared Setup Required

### Tailwind Config additions (`tailwind.config.cjs`):
```js
theme: {
  extend: {
    colors: {
      indigo: { 50: '#EEF2FF', 400: '#818CF8', 500: '#6366F1', 600: '#4F46E5', 700: '#4338CA' },
      green:  { 100: '#DCFCE7', 500: '#22C55E', 600: '#16A34A' },
      red:    { 500: '#EF4444' },
      neutral: {
        100: '#F1F5F9', 200: '#E2E8F0', 300: '#CBD5E1',
        400: '#94A3B8', 500: '#64748B', 700: '#334155',
        800: '#1E293B', 900: '#0F172A',
      },
    },
  },
},
```

### CSS base classes (add to your global CSS):
```css
/* These are used by TextInput, SelectInput, DOBPicker */
.form-input {
  @apply h-11 px-4 rounded-full border border-neutral-300 bg-white
         text-neutral-900 placeholder:text-neutral-400
         focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-300
         transition-all duration-200;
}
.form-input:disabled {
  @apply bg-neutral-100 text-neutral-500 cursor-not-allowed;
}
```

### npm packages:
```bash
npm install @radix-ui/react-select lucide-react
# or
pnpm add @radix-ui/react-select lucide-react
```
