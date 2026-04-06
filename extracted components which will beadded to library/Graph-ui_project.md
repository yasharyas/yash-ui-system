# Reusable Component Registry

Extracted from `graph-ui`. Five components identified as visually reusable, independent, and decoupled from app state/backend.

---

=== COMPONENT START ===

**Name:** ToastContainer + useToast

**Code:**
```tsx
import { useState, useCallback } from 'react';
import { createPortal } from 'react-dom';

interface Toast {
  id: number;
  message: string;
}

let _toastId = 0;

export function useToast() {
  const [toasts, setToasts] = useState<Toast[]>([]);

  const show = useCallback((message: string, duration = 2500) => {
    const id = ++_toastId;
    setToasts((prev) => [...prev, { id, message }]);
    setTimeout(() => {
      setToasts((prev) => prev.filter((t) => t.id !== id));
    }, duration);
  }, []);

  return { toasts, show };
}

export function ToastContainer({ toasts }: { toasts: Toast[] }) {
  if (toasts.length === 0) return null;
  return createPortal(
    <div className="fixed bottom-6 right-6 z-[9999] flex flex-col gap-2">
      {toasts.map((t) => (
        <div
          key={t.id}
          className="bg-neutral-800 text-white text-sm px-4 py-2.5 rounded-xl shadow-lg animate-[slideUp_0.2s_ease-out]"
        >
          {t.message}
        </div>
      ))}
    </div>,
    document.body
  );
}
```

**Props:**
```ts
// ToastContainer
type ToastContainerProps = {
  toasts: { id: number; message: string }[];
};

// useToast returns
type UseToastReturn = {
  toasts: { id: number; message: string }[];
  show: (message: string, duration?: number) => void;
};
```

**Dependencies:**
- `react` (useState, useCallback, createPortal)
- Tailwind CSS
- `slideUp` keyframe animation must be defined in your global CSS or Tailwind config:
  ```css
  @keyframes slideUp {
    from { opacity: 0; transform: translateY(8px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  ```

**Styles:**
- `fixed bottom-6 right-6 z-[9999] flex flex-col gap-2` — portal container
- `bg-neutral-800 text-white text-sm px-4 py-2.5 rounded-xl shadow-lg animate-[slideUp_0.2s_ease-out]` — individual toast

**Usage:**
```tsx
function App() {
  const { toasts, show } = useToast();
  return (
    <>
      <button onClick={() => show('Saved!')}>Save</button>
      <ToastContainer toasts={toasts} />
    </>
  );
}
```

**Prompt:**
> Create a React toast notification system with a `useToast` hook and `ToastContainer` component. Toasts should auto-dismiss after 2.5 seconds, stack vertically in the bottom-right corner using a portal, and animate in with a slide-up effect. Use Tailwind for styling with a dark neutral background.

**Registry JSON:**
```json
{
  "name": "ToastContainer",
  "category": "feedback",
  "tags": ["toast", "notification", "portal", "hook", "auto-dismiss", "animated"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

**Name:** ToolbarButton

**Code:**
```tsx
import type { ReactNode } from 'react';

type ToolbarButtonProps = {
  icon: ReactNode;
  onClick: () => void;
  disabled?: boolean;
  title?: string;
};

export function ToolbarButton({ icon, onClick, disabled, title }: ToolbarButtonProps) {
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      title={title}
      className="p-2 rounded-xl text-neutral-500 hover:bg-neutral-100 hover:text-neutral-700 disabled:opacity-30 disabled:hover:bg-transparent transition-colors cursor-pointer disabled:cursor-not-allowed"
    >
      {icon}
    </button>
  );
}
```

**Props:**
```ts
type ToolbarButtonProps = {
  icon: ReactNode;       // any icon element, e.g. <ZoomIn size={15} />
  onClick: () => void;
  disabled?: boolean;    // dims to 30% opacity, removes hover effect
  title?: string;        // native tooltip on hover
};
```

**Dependencies:**
- `react`
- Tailwind CSS

**Styles:**
- `p-2 rounded-xl` — padding and shape
- `text-neutral-500 hover:bg-neutral-100 hover:text-neutral-700` — idle/hover colours
- `disabled:opacity-30 disabled:hover:bg-transparent disabled:cursor-not-allowed` — disabled state
- `transition-colors` — smooth colour transition

**Usage:**
```tsx
import { ZoomIn } from 'lucide-react';
import { ToolbarButton } from './ToolbarButton';

<ToolbarButton
  icon={<ZoomIn size={15} />}
  onClick={() => zoomIn()}
  title="Zoom In"
/>

<ToolbarButton
  icon={<Undo2 size={15} />}
  onClick={undo}
  disabled={undoStack.length === 0}
  title="Undo (Ctrl+Z)"
/>
```

**Prompt:**
> Create a compact icon-only toolbar button in React with Tailwind. It should accept a ReactNode icon, onClick handler, optional disabled state (reduced opacity, no hover), and an optional tooltip title. Rounded corners, neutral colour scheme.

**Registry JSON:**
```json
{
  "name": "ToolbarButton",
  "category": "buttons",
  "tags": ["icon-button", "toolbar", "disabled-state", "tooltip", "neutral"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

**Name:** CollapsibleSidebar

**Code:**
```tsx
import { useState, type DragEvent, type ReactNode } from 'react';
import { Search, PanelLeftClose, PanelLeft } from 'lucide-react';

export type SidebarItem = {
  id: string;
  label: string;
  description?: string;
  color: string;           // hex colour for icon bg tint and icon colour
  icon: ReactNode;
  dragData?: string;       // value written to dataTransfer on drag
  dragKey?: string;        // dataTransfer key, defaults to 'application/sidebar-item'
};

export type SidebarCategory = {
  label: string;
  items: SidebarItem[];
};

type CollapsibleSidebarProps = {
  title?: string;
  categories: SidebarCategory[];
  searchPlaceholder?: string;
  dragTransferKey?: string;
};

export function CollapsibleSidebar({
  title = 'Items',
  categories,
  searchPlaceholder = 'Search...',
  dragTransferKey = 'application/sidebar-item',
}: CollapsibleSidebarProps) {
  const [search, setSearch] = useState('');
  const [collapsed, setCollapsed] = useState(false);

  const onDragStart = (e: DragEvent, item: SidebarItem) => {
    e.dataTransfer.setData(item.dragKey ?? dragTransferKey, item.dragData ?? item.id);
    e.dataTransfer.effectAllowed = 'move';
  };

  const filterItem = (item: SidebarItem) =>
    item.label.toLowerCase().includes(search.toLowerCase());

  if (collapsed) {
    return (
      <div className="w-12 bg-white border-r border-neutral-200 flex flex-col items-center pt-3">
        <button
          onClick={() => setCollapsed(false)}
          className="p-2 rounded-lg hover:bg-neutral-100 text-neutral-500 transition-colors cursor-pointer"
        >
          <PanelLeft size={18} />
        </button>
      </div>
    );
  }

  return (
    <div className="w-60 bg-white border-r border-neutral-200 flex flex-col h-full">
      {/* Header */}
      <div className="flex items-center justify-between px-4 pt-4 pb-2">
        <h2 className="text-xs font-semibold text-neutral-400 uppercase tracking-wider">{title}</h2>
        <button
          onClick={() => setCollapsed(true)}
          className="p-1.5 rounded-lg hover:bg-neutral-100 text-neutral-400 transition-colors cursor-pointer"
        >
          <PanelLeftClose size={16} />
        </button>
      </div>

      {/* Search */}
      <div className="px-3 pb-3">
        <div className="relative">
          <Search size={14} className="absolute left-2.5 top-1/2 -translate-y-1/2 text-neutral-400" />
          <input
            type="text"
            placeholder={searchPlaceholder}
            value={search}
            onChange={(e) => setSearch(e.target.value)}
            className="w-full pl-8 pr-3 py-2 text-xs bg-neutral-50 border border-neutral-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-neutral-300 placeholder-neutral-400 transition"
          />
        </div>
      </div>

      {/* Item list */}
      <div className="flex-1 overflow-y-auto px-3 pb-3 space-y-4">
        {categories.map((cat) => {
          const filtered = cat.items.filter(filterItem);
          if (filtered.length === 0) return null;
          return (
            <div key={cat.label}>
              <h3 className="text-[10px] font-semibold text-neutral-300 uppercase tracking-widest mb-2 px-1">
                {cat.label}
              </h3>
              <div className="space-y-1.5">
                {filtered.map((item) => (
                  <div
                    key={item.id}
                    draggable
                    onDragStart={(e) => onDragStart(e, item)}
                    className="flex items-center gap-2.5 px-3 py-2.5 rounded-xl bg-neutral-50 border border-neutral-100 hover:border-neutral-300 hover:shadow-sm cursor-grab active:cursor-grabbing transition-all group"
                  >
                    <div
                      className="flex items-center justify-center w-7 h-7 rounded-lg transition-transform group-hover:scale-110"
                      style={{ backgroundColor: `${item.color}18` }}
                    >
                      <span style={{ color: item.color, display: 'flex' }}>{item.icon}</span>
                    </div>
                    <div>
                      <div className="text-xs font-medium text-neutral-700">{item.label}</div>
                      {item.description && (
                        <div className="text-[10px] text-neutral-400">{item.description}</div>
                      )}
                    </div>
                  </div>
                ))}
              </div>
            </div>
          );
        })}
      </div>
    </div>
  );
}
```

**Props:**
```ts
type SidebarItem = {
  id: string;
  label: string;
  description?: string;
  color: string;        // hex, e.g. '#3b82f6'
  icon: ReactNode;
  dragData?: string;    // data written to dataTransfer (defaults to item.id)
  dragKey?: string;     // dataTransfer key (defaults to dragTransferKey prop)
};

type SidebarCategory = {
  label: string;
  items: SidebarItem[];
};

type CollapsibleSidebarProps = {
  title?: string;              // panel heading, default 'Items'
  categories: SidebarCategory[];
  searchPlaceholder?: string;
  dragTransferKey?: string;    // default 'application/sidebar-item'
};
```

**Dependencies:**
- `react`
- `lucide-react` (`Search`, `PanelLeftClose`, `PanelLeft`)
- Tailwind CSS

**Styles:**
- Collapsible: narrows from `w-60` to `w-12`
- Items have a colour-tinted icon container (`${color}18` = 10% opacity hex tint)
- Search has inset icon via `absolute` positioning

**Usage:**
```tsx
import { Zap, Play } from 'lucide-react';
import { CollapsibleSidebar } from './CollapsibleSidebar';

const categories = [
  {
    label: 'Triggers',
    items: [
      { id: 'webhook', label: 'Webhook', description: 'HTTP trigger', color: '#3b82f6', icon: <Zap size={14} />, dragData: 'webhook' },
    ],
  },
  {
    label: 'Actions',
    items: [
      { id: 'http', label: 'HTTP Request', description: 'Call an API', color: '#22c55e', icon: <Play size={14} />, dragData: 'http' },
    ],
  },
];

<CollapsibleSidebar
  title="Nodes"
  categories={categories}
  searchPlaceholder="Search nodes..."
  dragTransferKey="application/my-app-node"
/>
```

**Prompt:**
> Build a collapsible left sidebar in React + Tailwind. It should have a title, a search input with an inset icon, and a list of categorised draggable items. Each item has a coloured icon badge, a label, and an optional description. The sidebar should toggle between full width (240 px) and icon-only (48 px) using PanelLeft / PanelLeftClose icons. Items write data to HTML5 drag-and-drop dataTransfer on drag start.

**Registry JSON:**
```json
{
  "name": "CollapsibleSidebar",
  "category": "navigation",
  "tags": ["sidebar", "collapsible", "draggable", "searchable", "categorised", "panel"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

**Name:** SidePanel

**Code:**
```tsx
import type { ReactNode } from 'react';
import { X } from 'lucide-react';

type SidePanelProps = {
  title: string;
  headerLeft?: ReactNode;   // icon/badge area rendered left of title
  onClose: () => void;
  footer?: ReactNode;       // e.g. a delete button
  children: ReactNode;      // form fields
};

export function SidePanel({ title, headerLeft, onClose, footer, children }: SidePanelProps) {
  return (
    <div className="w-72 bg-white border-l border-neutral-200 flex flex-col h-full animate-[slideIn_0.15s_ease-out]">
      {/* Header */}
      <div className="flex items-center justify-between px-4 pt-4 pb-3 border-b border-neutral-100">
        <div className="flex items-center gap-2">
          {headerLeft}
          <span className="text-[10px] font-semibold text-neutral-400 uppercase tracking-wider">
            {title}
          </span>
        </div>
        <button
          onClick={onClose}
          className="p-1.5 rounded-lg hover:bg-neutral-100 text-neutral-400 transition-colors cursor-pointer"
        >
          <X size={16} />
        </button>
      </div>

      {/* Body */}
      <div className="flex-1 overflow-y-auto p-4 space-y-4">
        {children}
      </div>

      {/* Footer */}
      {footer && (
        <div className="p-4 border-t border-neutral-100">
          {footer}
        </div>
      )}
    </div>
  );
}

// ── Convenience sub-components ──────────────────────────────────────────────

type FieldProps = {
  label: string;
  children: ReactNode;
};

export function PanelField({ label, children }: FieldProps) {
  return (
    <div>
      <label className="text-[10px] font-semibold text-neutral-400 uppercase tracking-wider">
        {label}
      </label>
      <div className="mt-1">{children}</div>
    </div>
  );
}

export function PanelInput(props: React.InputHTMLAttributes<HTMLInputElement>) {
  return (
    <input
      {...props}
      className={
        'w-full px-3 py-2 text-sm bg-neutral-50 border border-neutral-200 rounded-xl ' +
        'focus:outline-none focus:ring-2 focus:ring-neutral-300 placeholder-neutral-300 transition ' +
        (props.className ?? '')
      }
    />
  );
}

export function PanelTextarea(props: React.TextareaHTMLAttributes<HTMLTextAreaElement>) {
  return (
    <textarea
      {...props}
      className={
        'w-full px-3 py-2 text-sm bg-neutral-50 border border-neutral-200 rounded-xl ' +
        'focus:outline-none focus:ring-2 focus:ring-neutral-300 resize-none transition ' +
        (props.className ?? '')
      }
    />
  );
}

export function PanelDeleteButton({ onClick, label = 'Delete' }: { onClick: () => void; label?: string }) {
  return (
    <button
      onClick={onClick}
      className="w-full flex items-center justify-center gap-2 py-2 text-sm text-red-500 hover:bg-red-50 rounded-xl transition-colors cursor-pointer"
    >
      {label}
    </button>
  );
}
```

**Props:**
```ts
type SidePanelProps = {
  title: string;
  headerLeft?: ReactNode;  // optional icon/badge on the left of the header
  onClose: () => void;
  footer?: ReactNode;      // renders below a divider at the bottom
  children: ReactNode;
};

// Sub-components (optional convenience wrappers)
PanelField   — wraps a labelled form row
PanelInput   — styled <input>
PanelTextarea — styled <textarea>
PanelDeleteButton — full-width red action button in the footer
```

**Dependencies:**
- `react`
- `lucide-react` (`X`)
- Tailwind CSS
- `slideIn` keyframe must be defined:
  ```css
  @keyframes slideIn {
    from { opacity: 0; transform: translateX(12px); }
    to   { opacity: 1; transform: translateX(0); }
  }
  ```

**Styles:**
- `w-72` fixed width, full height, left border
- `animate-[slideIn_0.15s_ease-out]` entrance animation
- Body is `overflow-y-auto` with `space-y-4` form spacing

**Usage:**
```tsx
import { SidePanel, PanelField, PanelInput, PanelTextarea, PanelDeleteButton } from './SidePanel';
import { Zap } from 'lucide-react';

<SidePanel
  title="Edit Node"
  headerLeft={
    <div className="w-8 h-8 rounded-xl flex items-center justify-center" style={{ backgroundColor: '#3b82f618' }}>
      <Zap size={16} style={{ color: '#3b82f6' }} />
    </div>
  }
  onClose={() => setOpen(false)}
  footer={<PanelDeleteButton onClick={handleDelete} label="Delete Node" />}
>
  <PanelField label="Label">
    <PanelInput value={label} onChange={(e) => setLabel(e.target.value)} />
  </PanelField>
  <PanelField label="Description">
    <PanelTextarea value={desc} onChange={(e) => setDesc(e.target.value)} rows={3} />
  </PanelField>
</SidePanel>
```

**Prompt:**
> Create a right-side sliding panel component in React + Tailwind with a header (title, optional left icon slot, close button), a scrollable body for form fields, and an optional sticky footer. Include convenience sub-components: a labelled field wrapper, a styled input, a styled textarea, and a full-width red delete button. Add a slide-in-from-right entrance animation.

**Registry JSON:**
```json
{
  "name": "SidePanel",
  "category": "panels",
  "tags": ["side-panel", "drawer", "form", "editor", "slide-in", "animated"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

**Name:** NodeCard

**Code:**
```tsx
import type { ReactNode, CSSProperties } from 'react';

type NodeCardProps = {
  label: string;
  description?: string;
  icon: ReactNode;
  accentColor: string;     // hex, e.g. '#3b82f6'
  selected?: boolean;
  onClick?: () => void;
  // React Flow handle slots — pass <Handle> elements if using inside ReactFlow
  topHandle?: ReactNode;
  bottomHandle?: ReactNode;
};

export function NodeCard({
  label,
  description,
  icon,
  accentColor,
  selected = false,
  onClick,
  topHandle,
  bottomHandle,
}: NodeCardProps) {
  return (
    <div
      className={[
        'relative min-w-[180px] max-w-[240px] rounded-2xl bg-white',
        'border-2 transition-all duration-150 cursor-pointer',
        selected ? 'ring-2 ring-offset-2 shadow-md' : 'shadow-sm hover:shadow-md',
      ].join(' ')}
      style={{
        borderColor: selected ? accentColor : '#e2e8f0',
        ...(selected
          ? ({ '--tw-ring-color': accentColor } as CSSProperties)
          : {}),
      }}
      onClick={onClick}
    >
      {/* Left colour accent bar */}
      <div
        className="absolute left-0 top-3 bottom-3 w-1 rounded-full"
        style={{ backgroundColor: accentColor }}
      />

      {/* Header */}
      <div className="flex items-center gap-2 px-4 pt-3 pb-1">
        <div
          className="flex items-center justify-center w-7 h-7 rounded-lg"
          style={{ backgroundColor: `${accentColor}18` }}
        >
          <span style={{ color: accentColor, display: 'flex' }}>{icon}</span>
        </div>
        <span className="text-sm font-semibold text-neutral-800 truncate">{label}</span>
      </div>

      {/* Description */}
      {description && (
        <div className="px-4 pb-3">
          <p className="text-xs text-neutral-400 truncate">{description}</p>
        </div>
      )}

      {/* Handle slots */}
      {topHandle}
      {bottomHandle}
    </div>
  );
}
```

**Props:**
```ts
type NodeCardProps = {
  label: string;
  description?: string;
  icon: ReactNode;         // any icon element, sized and coloured via accentColor
  accentColor: string;     // hex colour drives border, icon bg tint, accent bar
  selected?: boolean;      // shows ring + coloured border when true
  onClick?: () => void;
  topHandle?: ReactNode;   // pass <Handle type="target" .../> from @xyflow/react
  bottomHandle?: ReactNode;// pass <Handle type="source" .../> from @xyflow/react
};
```

**Dependencies:**
- `react`
- Tailwind CSS
- _Optional_: `@xyflow/react` only if you pass `<Handle>` elements via `topHandle`/`bottomHandle`

**Styles:**
- `rounded-2xl border-2` card shape
- `ring-2 ring-offset-2` selected state ring (colour driven by CSS variable)
- `absolute left-0 top-3 bottom-3 w-1 rounded-full` — coloured left accent bar
- `${accentColor}18` — 10% opacity hex tint for icon background

**Usage:**

Without React Flow (standalone card):
```tsx
import { Zap } from 'lucide-react';
import { NodeCard } from './NodeCard';

<NodeCard
  label="Send Email"
  description="Sends a templated email"
  icon={<Zap size={15} />}
  accentColor="#3b82f6"
  selected={isSelected}
  onClick={() => setSelected(id)}
/>
```

With React Flow handles:
```tsx
import { Handle, Position } from '@xyflow/react';

<NodeCard
  label="Trigger"
  icon={<Zap size={15} />}
  accentColor="#3b82f6"
  selected={selected}
  onClick={() => onSelect(id)}
  topHandle={
    <Handle
      type="target"
      position={Position.Top}
      className="w-3! h-3! rounded-full! border-2! border-white! bg-neutral-300! hover:bg-neutral-500! -top-1.5!"
    />
  }
  bottomHandle={
    <Handle
      type="source"
      position={Position.Bottom}
      className="w-3! h-3! rounded-full! border-2! border-white! bg-neutral-300! hover:bg-neutral-500! -bottom-1.5!"
    />
  }
/>
```

**Prompt:**
> Create a React + Tailwind node card component for a visual workflow builder. It should have a coloured left accent bar, a tinted icon badge, a label, and an optional description. Drive all colours from a single `accentColor` hex prop. Show a ring highlight when `selected` is true. Accept optional slot props for top/bottom connection handles so it can be used with React Flow. Min width 180 px, max 240 px.

**Registry JSON:**
```json
{
  "name": "NodeCard",
  "category": "cards",
  "tags": ["node", "card", "workflow", "react-flow", "accent-color", "selectable", "draggable"]
}
```

=== COMPONENT END ===
