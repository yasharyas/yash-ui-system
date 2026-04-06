Created 3 todos

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui)

Read [](file:///home/user/Documents/Work/Myanthology/src/features)

Read [](file:///home/user/Documents/Work/Myanthology/src/shared/components)

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/tubelight-navbar.tsx#1-1), lines 1 to 120

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/material-design-3-switch.tsx#1-1), lines 1 to 120

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/DualConfirmDialog.tsx#1-1), lines 1 to 150

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/BlenderUpload.tsx#1-1), lines 1 to 120

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/empty-state.tsx)

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/pagination.tsx)

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/BlenderUpload.tsx#120-120), lines 120 to 200

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/material-design-3-switch.tsx#120-120), lines 120 to 248

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/custom-checkbox.tsx)

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/BlenderUpload.tsx#200-200), lines 200 to 200

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/tubelight-navbar.tsx#110-110), lines 110 to 120

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/DualConfirmDialog.tsx#145-145), lines 145 to 150

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/BlenderUpload.tsx#200-200), lines 200 to 350

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/DualConfirmDialog.tsx#145-145), lines 145 to 166

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/BlenderUpload.tsx#340-340), lines 340 to 430

Read [](file:///home/user/Documents/Work/Myanthology/src/shared/components/LoadingSpinner.tsx)

Read [](file:///home/user/Documents/Work/Myanthology/src/shared/components/PriceBreakdown.tsx)

Read [](file:///home/user/Documents/Work/Myanthology/src/lib/utils.ts)

Read [](file:///home/user/Documents/Work/Myanthology/src/components/ui/badge.tsx)

Starting: *Extract reusable components with full specs* (3/3)

Here is the full component extraction report:

---

# Reusable Component Extraction Report

---

## =========================
## === COMPONENT START ===
## =========================

**Name:** `TubelightNavBar`

**Code:**
```tsx
"use client"

import { useEffect, useState } from "react"
import { motion } from "framer-motion"
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"
import type { LucideIcon } from "lucide-react"

function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

export interface NavItem {
  name: string
  url: string
  icon: LucideIcon
  onClick?: () => void
}

interface NavBarProps {
  items: NavItem[]
  activeItem?: string
  className?: string
  onNavigate?: (url: string) => void
}

export function TubelightNavBar({ items, activeItem, className, onNavigate }: NavBarProps) {
  const [isMobile, setIsMobile] = useState(false)

  const currentActive = activeItem ?? items[0]?.name ?? ""

  useEffect(() => {
    const handleResize = () => setIsMobile(window.innerWidth < 768)
    handleResize()
    window.addEventListener("resize", handleResize)
    return () => window.removeEventListener("resize", handleResize)
  }, [])

  return (
    <div
      className={cn(
        "fixed bottom-0 sm:top-0 left-1/2 -translate-x-1/2 z-50 mb-3 sm:mb-0 sm:pt-3 pointer-events-none",
        className,
      )}
    >
      <div className="flex items-center gap-1 bg-background/5 border border-border backdrop-blur-lg py-0.5 px-0.5 rounded-full shadow-lg pointer-events-auto">
        {items.map((item) => {
          const Icon = item.icon
          const isActive = currentActive === item.name
          const itemClassName = cn(
            "relative cursor-pointer text-xs font-semibold px-3 py-1.5 rounded-full transition-colors text-center",
            "text-foreground/80 hover:text-primary",
            isActive && "bg-muted text-primary",
          )
          const content = (
            <>
              <span className={isMobile ? "hidden" : "hidden md:inline"}>{item.name}</span>
              <span className={isMobile ? "inline" : "md:hidden"}>
                <Icon size={14} strokeWidth={2.5} />
              </span>
              {isActive && (
                <motion.div
                  layoutId="lamp"
                  className="absolute inset-0 w-full bg-primary/5 rounded-full -z-10"
                  initial={false}
                  transition={{ type: "spring", stiffness: 300, damping: 30 }}
                >
                  <div className="absolute -top-1.5 left-1/2 -translate-x-1/2 w-6 h-0.5 bg-primary rounded-t-full">
                    <div className="absolute w-8 h-4 bg-primary/20 rounded-full blur-md -top-1.5 -left-1" />
                    <div className="absolute w-6 h-4 bg-primary/20 rounded-full blur-md -top-0.5" />
                    <div className="absolute w-3 h-3 bg-primary/20 rounded-full blur-sm top-0 left-1.5" />
                  </div>
                </motion.div>
              )}
            </>
          )

          if (item.onClick) {
            return (
              <button key={item.name} onClick={item.onClick} className={itemClassName}>
                {content}
              </button>
            )
          }

          return (
            <a
              key={item.name}
              href={item.url}
              onClick={(e) => { if (onNavigate) { e.preventDefault(); onNavigate(item.url) } }}
              className={itemClassName}
            >
              {content}
            </a>
          )
        })}
      </div>
    </div>
  )
}
```

**Props:**
```typescript
type NavItem = {
  name: string        // Display label
  url: string         // href / route path
  icon: LucideIcon    // Lucide icon component
  onClick?: () => void  // Optional click override (renders as <button>)
}

type NavBarProps = {
  items: NavItem[]
  activeItem?: string       // Name of the currently active item
  className?: string
  onNavigate?: (url: string) => void  // SPA navigation callback
}
```

**Dependencies:**
```
framer-motion
lucide-react
clsx
tailwind-merge
tailwindcss (with primary, background, border, muted CSS vars)
```

**Styles:**
Tailwind utility classes. Requires CSS variable tokens: `--primary`, `--background`, `--border`, `--muted`, `--foreground`. Uses `backdrop-blur-lg`, spring animations via `framer-motion`.

**Usage:**
```tsx
import { Home, Calendar, Settings } from "lucide-react"

const navItems = [
  { name: "Home", url: "/", icon: Home },
  { name: "Events", url: "/events", icon: Calendar },
  { name: "Settings", url: "/settings", icon: Settings },
]

<TubelightNavBar
  items={navItems}
  activeItem="Events"
  onNavigate={(url) => router.push(url)}
/>
```

**Prompt:**
> "Create a floating pill-shaped navigation bar fixed to the bottom on mobile and top on desktop. Each nav item shows the icon on mobile and text label on desktop. The active item has a glowing tubelight/lamp effect above it, animated with framer-motion spring. Use Tailwind CSS with theme tokens."

**Registry JSON:**
```json
{
  "name": "TubelightNavBar",
  "category": "navigation",
  "tags": ["navbar", "floating", "animated", "tubelight", "pill", "framer-motion", "responsive"]
}
```

## =========================
## === COMPONENT END ===
## =========================

---

## =========================
## === COMPONENT START ===
## =========================

**Name:** `MD3Switch` (Material Design 3 Switch)

**Code:**
```tsx
import * as React from "react"
import { cva, type VariantProps } from "class-variance-authority"
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"
import { Check, X } from "lucide-react"

function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

const SWITCH_THEME = {
  "--ease-spring": "cubic-bezier(0.175, 0.885, 0.32, 1.275)",
} as React.CSSProperties

const switchVariants = cva(
  "peer inline-flex shrink-0 cursor-pointer items-center rounded-full border-2 transition-colors duration-300 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background disabled:cursor-not-allowed disabled:opacity-50",
  {
    variants: {
      variant: { primary: "", destructive: "" },
      size: {
        default: "h-8 w-[52px]",
        sm: "h-6 w-10",
      },
    },
    defaultVariants: { variant: "primary", size: "default" },
  }
)

const playHapticFeedback = (type: "heavy" | "light" | "none") => {
  if (type === "none" || typeof window === "undefined") return
  try {
    const AudioContext = window.AudioContext || (window as any).webkitAudioContext
    if (!AudioContext) return
    const ctx = new AudioContext()
    const oscillator = ctx.createOscillator()
    const gainNode = ctx.createGain()
    oscillator.connect(gainNode)
    gainNode.connect(ctx.destination)
    const now = ctx.currentTime
    if (type === "heavy") {
      oscillator.type = "triangle"
      oscillator.frequency.setValueAtTime(180, now)
      oscillator.frequency.exponentialRampToValueAtTime(40, now + 0.15)
      gainNode.gain.setValueAtTime(0.4, now)
      gainNode.gain.exponentialRampToValueAtTime(0.01, now + 0.12)
      oscillator.start(now); oscillator.stop(now + 0.15)
    } else {
      oscillator.type = "sine"
      oscillator.frequency.setValueAtTime(800, now)
      gainNode.gain.setValueAtTime(0.15, now)
      gainNode.gain.exponentialRampToValueAtTime(0.01, now + 0.08)
      oscillator.start(now); oscillator.stop(now + 0.08)
    }
  } catch { /* silent fail */ }
}

export interface MD3SwitchProps
  extends Omit<React.InputHTMLAttributes<HTMLInputElement>, "size">,
    VariantProps<typeof switchVariants> {
  onCheckedChange?: (checked: boolean) => void
  showIcons?: boolean
  checkedIcon?: React.ReactNode
  uncheckedIcon?: React.ReactNode
  haptic?: "heavy" | "light" | "none"
}

export const MD3Switch = React.forwardRef<HTMLInputElement, MD3SwitchProps>(
  ({
    className, size, variant, checked, defaultChecked,
    onCheckedChange, showIcons = false, checkedIcon, uncheckedIcon,
    haptic = "none", style, disabled, ...props
  }, ref) => {
    const [isChecked, setIsChecked] = React.useState(defaultChecked ?? false)
    const [isPressed, setIsPressed] = React.useState(false)
    const [isHovered, setIsHovered] = React.useState(false)

    React.useEffect(() => {
      if (checked !== undefined) setIsChecked(checked)
    }, [checked])

    const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
      if (disabled) return
      const newValue = e.target.checked
      playHapticFeedback(haptic)
      if (checked === undefined) setIsChecked(newValue)
      onCheckedChange?.(newValue)
    }

    const isSmall = size === "sm"
    const translateDist = isSmall ? "translate-x-[16px]" : "translate-x-[20px]"
    const handleSizeUnchecked = isSmall ? "w-3 h-3 ml-[2px]" : "w-4 h-4 ml-[2px]"
    const handleSizeChecked = isSmall ? "w-4 h-4" : "w-6 h-6"
    const handleSizePressed = isSmall ? "w-5 h-5 -ml-[2px]" : "w-7 h-7 -ml-[2px]"
    const iconClasses = isSmall ? "w-2.5 h-2.5" : "w-3.5 h-3.5"
    const shouldRenderIcons = showIcons || checkedIcon || uncheckedIcon

    return (
      <label
        className={cn(
          "group relative inline-flex items-center justify-center",
          disabled && "cursor-not-allowed opacity-50",
          "min-w-[48px] min-h-[48px]"
        )}
        style={{ ...SWITCH_THEME, ...style }}
        onPointerDown={() => !disabled && setIsPressed(true)}
        onPointerUp={() => setIsPressed(false)}
        onPointerLeave={() => { setIsPressed(false); setIsHovered(false) }}
        onPointerEnter={() => !disabled && setIsHovered(true)}
      >
        <input type="checkbox" className="peer sr-only" ref={ref}
          checked={isChecked} onChange={handleChange} disabled={disabled} {...props} />

        <div className={cn(switchVariants({ variant, size }),
          "bg-muted border-border",
          "peer-checked:bg-primary peer-checked:border-primary", className)}>
          <div className={cn("pointer-events-none block h-full w-full transition-all duration-300 ease-[var(--ease-spring)]",
            isChecked ? translateDist : "translate-x-0")}>
            <div className={cn(
              "absolute top-1/2 -translate-y-1/2 shadow-sm transition-all duration-300 flex items-center justify-center rounded-full left-[2px]",
              isChecked ? "bg-primary-foreground" : "bg-foreground text-muted",
              isChecked && variant === "primary" && "text-primary",
              isChecked && variant === "destructive" && "text-destructive",
              isPressed ? handleSizePressed
                : isChecked || (shouldRenderIcons && !isSmall) ? handleSizeChecked
                : handleSizeUnchecked
            )}>
              {shouldRenderIcons && (
                <div className="relative w-full h-full flex items-center justify-center overflow-hidden">
                  <div className={cn("absolute inset-0 flex items-center justify-center transition-all duration-300",
                    isChecked ? "opacity-100 scale-100 rotate-0" : "opacity-0 scale-50 -rotate-45")}>
                    {checkedIcon ?? <Check className={iconClasses} strokeWidth={4} />}
                  </div>
                  <div className={cn("absolute inset-0 flex items-center justify-center transition-all duration-300 text-muted-foreground",
                    !isChecked ? "opacity-100 scale-100 rotate-0" : "opacity-0 scale-50 rotate-45")}>
                    {uncheckedIcon ?? <X className={iconClasses} strokeWidth={4} />}
                  </div>
                </div>
              )}
            </div>
            <div className={cn(
              "absolute top-1/2 left-[2px] -translate-y-1/2 -translate-x-1/2 rounded-full pointer-events-none transition-all duration-200",
              isSmall ? "w-8 h-8" : "w-10 h-10",
              isChecked ? (variant === "destructive" ? "bg-destructive" : "bg-primary") : "bg-foreground",
              isPressed ? "opacity-10 scale-100" : isHovered ? "opacity-5 scale-100" : "opacity-0 scale-50",
              isChecked ? "left-[14px]" : (shouldRenderIcons && !isSmall) ? "left-[14px]" : "left-[10px]",
              isSmall && (isChecked ? "left-[10px]" : "left-[8px]")
            )} />
          </div>
        </div>
      </label>
    )
  }
)
MD3Switch.displayName = "MD3Switch"
```

**Props:**
```typescript
type MD3SwitchProps = {
  checked?: boolean
  defaultChecked?: boolean
  onCheckedChange?: (checked: boolean) => void
  variant?: "primary" | "destructive"
  size?: "default" | "sm"
  showIcons?: boolean          // Show default check/X icons
  checkedIcon?: React.ReactNode   // Custom icon when on
  uncheckedIcon?: React.ReactNode // Custom icon when off
  haptic?: "heavy" | "light" | "none"  // Web Audio API click feedback
  disabled?: boolean
  className?: string
  style?: React.CSSProperties
}
```

**Dependencies:**
```
class-variance-authority
lucide-react (Check, X icons)
clsx
tailwind-merge
tailwindcss (primary, muted, destructive, foreground, ring CSS vars)
```

**Styles:**
Tailwind utility classes + CSS custom property `--ease-spring` for spring easing. Requires theme tokens: `--primary`, `--muted`, `--destructive`, `--foreground`, `--ring`.

**Usage:**
```tsx
// Basic
<MD3Switch onCheckedChange={(v) => console.log(v)} />

// With icons, destructive, haptic
<MD3Switch
  variant="destructive"
  size="sm"
  showIcons
  haptic="heavy"
  defaultChecked
  onCheckedChange={(v) => setEnabled(v)}
/>

// Custom icons
<MD3Switch
  checkedIcon={<Sun className="w-3 h-3" />}
  uncheckedIcon={<Moon className="w-3 h-3" />}
/>
```

**Prompt:**
> "Build a Material Design 3 toggle switch in React with spring-easing physics for the handle, a hover/press halo effect, optional check/X icons that rotate in/out, two sizes (default and sm), primary and destructive color variants, and an optional Web Audio API haptic feedback click sound."

**Registry JSON:**
```json
{
  "name": "MD3Switch",
  "category": "forms",
  "tags": ["switch", "toggle", "material-design", "md3", "animated", "haptic", "physics"]
}
```

## =========================
## === COMPONENT END ===
## =========================

---

## =========================
## === COMPONENT START ===
## =========================

**Name:** `DualConfirmDialog`

**Code:**
```tsx
import { useState } from "react"
import {
  Dialog, DialogContent, DialogDescription,
  DialogFooter, DialogHeader, DialogTitle,
} from "./dialog"               // shadcn/ui dialog
import { Button } from "./button" // shadcn/ui button
import { Input } from "./input"   // shadcn/ui input
import { AlertTriangle, Loader2 } from "lucide-react"

export interface DeleteProgress {
  current: number
  total: number
  strategy?: "frontend" | "backend"
}

export interface DualConfirmDialogProps {
  open: boolean
  onOpenChange: (open: boolean) => void
  onConfirm: () => void
  title: string
  description: string
  itemCount: number
  itemType: string
  confirmationPhrase?: string
  isLoading?: boolean
  progress?: DeleteProgress | null
}

export function DualConfirmDialog({
  open, onOpenChange, onConfirm,
  title, description,
  itemCount, itemType,
  confirmationPhrase = "DELETE",
  isLoading = false,
  progress = null,
}: DualConfirmDialogProps) {
  const [step, setStep] = useState<1 | 2>(1)
  const [inputValue, setInputValue] = useState("")

  const handleFirstConfirm = () => setStep(2)
  const handleFinalConfirm = () => { if (inputValue === confirmationPhrase) onConfirm() }
  const handleClose = () => {
    if (isLoading) return
    setStep(1); setInputValue(""); onOpenChange(false)
  }
  const progressPercentage = progress
    ? Math.round((progress.current / progress.total) * 100) : 0

  return (
    <Dialog open={open} onOpenChange={handleClose}>
      <DialogContent className="sm:max-w-md">
        <DialogHeader>
          <DialogTitle className="flex items-center gap-2 text-destructive">
            <AlertTriangle className="h-5 w-5" />
            {title}
          </DialogTitle>
        </DialogHeader>

        {isLoading && progress ? (
          <div className="py-6 space-y-4">
            <div className="flex items-center justify-center gap-2">
              <Loader2 className="h-5 w-5 animate-spin" />
              <span>Processing...</span>
            </div>
            <div className="w-full bg-secondary rounded-full h-2.5">
              <div className="bg-primary h-2.5 rounded-full transition-all duration-300"
                style={{ width: `${progressPercentage}%` }} />
            </div>
            <p className="text-center text-sm text-muted-foreground">
              {progress.current} of {progress.total}
              {progress.strategy === "frontend" && " (sequential mode)"}
              {progress.strategy === "backend" && " (batch mode)"}
            </p>
          </div>
        ) : step === 1 ? (
          <>
            <DialogDescription asChild>
              <div className="py-4">
                <p className="text-muted-foreground">{description}</p>
                <div className="mt-4 p-3 bg-destructive/10 rounded-md border border-destructive/20">
                  <p className="font-semibold text-destructive">
                    You are about to delete {itemCount} {itemType}{itemCount > 1 ? "s" : ""}.
                  </p>
                  <p className="text-sm text-muted-foreground mt-1">This action cannot be undone.</p>
                </div>
              </div>
            </DialogDescription>
            <DialogFooter className="gap-2 sm:gap-0">
              <Button variant="outline" onClick={handleClose}>Cancel</Button>
              <Button variant="destructive" onClick={handleFirstConfirm}>
                Continue to Final Confirmation
              </Button>
            </DialogFooter>
          </>
        ) : (
          <>
            <DialogDescription asChild>
              <div className="py-4 space-y-4">
                <p className="text-destructive font-medium">⚠️ Final Confirmation Required</p>
                <p className="text-muted-foreground">
                  Type <code className="bg-muted px-2 py-1 rounded font-mono text-foreground">
                    {confirmationPhrase}
                  </code> to confirm deletion of {itemCount} {itemType}{itemCount > 1 ? "s" : ""}.
                </p>
                <Input
                  value={inputValue}
                  onChange={(e) => setInputValue(e.target.value)}
                  placeholder={`Type ${confirmationPhrase} to confirm`}
                  className="font-mono uppercase"
                  autoFocus
                  disabled={isLoading}
                  onKeyDown={(e) => {
                    if (e.key === "Enter" && inputValue === confirmationPhrase) handleFinalConfirm()
                  }}
                />
                <p className="text-xs text-muted-foreground">Note: Type in UPPERCASE letters</p>
              </div>
            </DialogDescription>
            <DialogFooter className="gap-2 sm:gap-0">
              <Button variant="outline" onClick={() => setStep(1)} disabled={isLoading}>Go Back</Button>
              <Button variant="destructive" onClick={handleFinalConfirm}
                disabled={inputValue !== confirmationPhrase || isLoading}>
                Delete Permanently
              </Button>
            </DialogFooter>
          </>
        )}
      </DialogContent>
    </Dialog>
  )
}
```

**Props:**
```typescript
type DeleteProgress = {
  current: number
  total: number
  strategy?: "frontend" | "backend"
}

type DualConfirmDialogProps = {
  open: boolean
  onOpenChange: (open: boolean) => void
  onConfirm: () => void
  title: string
  description: string
  itemCount: number
  itemType: string                  // e.g. "event", "user"
  confirmationPhrase?: string       // default: "DELETE"
  isLoading?: boolean
  progress?: DeleteProgress | null  // shows progress bar when deleting
}
```

**Dependencies:**
```
shadcn/ui: dialog, button, input
lucide-react (AlertTriangle, Loader2)
tailwindcss (destructive, muted, secondary, primary CSS vars)
```

**Styles:**
Tailwind utility classes. Uses `bg-destructive/10`, `border-destructive/20`. Requires standard shadcn/ui theme tokens.

**Usage:**
```tsx
const [open, setOpen] = useState(false)

<DualConfirmDialog
  open={open}
  onOpenChange={setOpen}
  onConfirm={() => handleBulkDelete()}
  title="Delete Items"
  description="Are you sure you want to delete the selected items?"
  itemCount={5}
  itemType="record"
  confirmationPhrase="DELETE"
  isLoading={isDeleting}
  progress={deleteProgress}
/>
```

**Prompt:**
> "Create a two-step destructive confirmation dialog using shadcn/ui. Step 1 shows a warning with item count. Step 2 requires the user to type a specific phrase (e.g. DELETE) to enable the final button. Include a loading state with an animated progress bar. Clicking outside is blocked while loading."

**Registry JSON:**
```json
{
  "name": "DualConfirmDialog",
  "category": "dialogs",
  "tags": ["confirmation", "destructive", "two-step", "modal", "bulk-delete", "loading", "progress"]
}
```

## =========================
## === COMPONENT END ===
## =========================

---

## =========================
## === COMPONENT START ===
## =========================

**Name:** `BlenderUpload`

**Code:**
```tsx
import { useCallback, useState, useRef } from "react"

interface BlenderUploadProps {
  onFileSelect: (file: File, dataUrl: string) => void
  onError?: (message: string) => void
  accept?: string
  maxSizeMB?: number
  disabled?: boolean
}

export function BlenderUpload({
  onFileSelect,
  onError,
  accept = ".jpg,.jpeg,.png",
  maxSizeMB = 1,
  disabled = false,
}: BlenderUploadProps) {
  const fileInputRef = useRef<HTMLInputElement>(null)
  const [isDragging, setIsDragging] = useState(false)
  const [isBlending, setIsBlending] = useState(false)
  const [blendComplete, setBlendComplete] = useState(false)
  const [previewUrl, setPreviewUrl] = useState("")

  const processFile = useCallback(async (file: File) => {
    if (disabled) return
    const VALID_TYPES = ["image/jpeg", "image/jpg", "image/png"]
    if (!VALID_TYPES.includes(file.type)) {
      onError?.("Only .jpg, .jpeg, .png files are allowed"); return
    }
    if (file.size > maxSizeMB * 1024 * 1024) {
      onError?.(`Image must be less than ${maxSizeMB} MB`); return
    }
    setIsBlending(true); setBlendComplete(false); setPreviewUrl("")
    const reader = new FileReader()
    reader.onload = (e) => {
      const dataUrl = e.target?.result as string
      setTimeout(() => {
        setIsBlending(false); setBlendComplete(true); setPreviewUrl(dataUrl)
        onFileSelect(file, dataUrl)
      }, 2000)
    }
    reader.onerror = () => { setIsBlending(false); onError?.("Failed to read file") }
    reader.readAsDataURL(file)
  }, [disabled, maxSizeMB, onError, onFileSelect])

  const handleDrop = useCallback((e: React.DragEvent) => {
    e.preventDefault(); setIsDragging(false)
    if (disabled || isBlending || blendComplete) return
    const file = e.dataTransfer.files[0]; if (file) processFile(file)
  }, [disabled, isBlending, blendComplete, processFile])

  const resetUpload = useCallback(() => {
    setBlendComplete(false); setPreviewUrl("")
  }, [])

  return (
    <div
      className={`relative overflow-hidden rounded-xl transition-all duration-300 ${isDragging ? "scale-[1.02]" : ""} ${disabled ? "opacity-50 cursor-not-allowed" : blendComplete ? "cursor-default" : "cursor-pointer"}`}
      onDrop={handleDrop}
      onDragOver={(e) => { e.preventDefault(); if (!disabled && !isBlending && !blendComplete) setIsDragging(true) }}
      onDragLeave={(e) => { e.preventDefault(); setIsDragging(false) }}
      onClick={() => { if (!disabled && !isBlending && !blendComplete) fileInputRef.current?.click() }}
      style={{ background: "#FFFFFF" }}
    >
      <input ref={fileInputRef} type="file" accept={accept} className="hidden"
        onChange={(e) => { const f = e.target.files?.[0]; if (f) processFile(f); e.target.value = "" }}
        disabled={disabled} />

      <div className={`relative p-6 m-3 border-2 border-dashed rounded-lg transition-colors ${isDragging ? "border-[#92A086] bg-[#92A086]/5" : blendComplete ? "border-[#92A086]" : "border-gray-300"}`}
        style={{ background: "#FFFFFF" }}>
        <div className="flex flex-col items-center justify-center">

          {/* ── BLENDER SVG (idle + blending state) ── */}
          {!blendComplete && (
            <svg viewBox="0 0 200 260" className="w-44 h-56">
              {/* Fruits (hidden when blending) */}
              {!isBlending && (
                <>
                  <g className={isDragging ? "animate-fruit-bounce" : ""} style={{ animationDelay: "0s" }}>
                    <circle cx="25" cy="35" r="14" fill="#F97316" />
                    <ellipse cx="21" cy="31" rx="4" ry="5" fill="#FDBA74" opacity="0.5" />
                    <circle cx="25" cy="24" r="3" fill="#92A086" />
                  </g>
                  {/* strawberry */}
                  <g className={isDragging ? "animate-fruit-bounce" : ""} style={{ animationDelay: "0.15s" }}>
                    <path d="M160 28 Q173 32, 177 48 Q179 64, 169 72 Q160 76, 151 72 Q141 64, 143 48 Q147 32, 160 28" fill="#DC2626" />
                    <path d="M160 28 Q160 18, 168 14 Q165 22, 160 28" fill="#92A086" />
                  </g>
                </>
              )}
              {/* Blender body */}
              <path d="M55 85 L50 195 Q50 210, 70 210 L130 210 Q150 210, 150 195 L145 85 Z" fill="#FFFFFF" stroke="#92A086" strokeWidth="2" />
              {isBlending && (
                <path d="M54 130 L52 195 Q52 205, 70 205 L130 205 Q148 205, 148 195 L146 130 Z" fill="#92A086" opacity="0.7" className="animate-liquid-swirl" />
              )}
              <path d="M50 105 Q20 105, 20 135 L20 165 Q20 185, 40 185 L50 185" fill="none" stroke="#92A086" strokeWidth="10" strokeLinecap="round" />
              <rect x="60" y="210" width="80" height="20" rx="4" fill="#92A086" />
            </svg>
          )}

          {/* ── SMOOTHIE GLASS SVG (complete state) ── */}
          {blendComplete && (
            <svg viewBox="0 0 160 200" className="w-40 h-52 animate-glass-appear">
              <rect x="95" y="10" width="6" height="120" rx="3" fill="#92A086" />
              <path d="M35 50 L30 160 Q30 175, 50 175 L110 175 Q130 175, 130 160 L125 50 Z" fill="url(#smoothieGradient)" stroke="#92A086" strokeWidth="2" />
              <ellipse cx="80" cy="45" rx="30" ry="12" fill="white" />
              <circle cx="80" cy="32" r="10" fill="#DC2626" />
              <path d="M80 22 Q82 15, 88 12" stroke="#92A086" strokeWidth="2" fill="none" />
              <defs>
                <linearGradient id="smoothieGradient" x1="0%" y1="0%" x2="0%" y2="100%">
                  <stop offset="0%" stopColor="#B8C4AC" />
                  <stop offset="50%" stopColor="#92A086" />
                  <stop offset="100%" stopColor="#7A8A70" />
                </linearGradient>
              </defs>
            </svg>
          )}

          {/* Text */}
          <div className="mt-4 text-center">
            <h3 className={`text-xl font-bold transition-colors ${isBlending ? "text-[#7A8A70]" : blendComplete ? "text-[#92A086]" : "text-gray-800"}`}>
              {isBlending ? "Uploading..." : blendComplete ? "🍹 Smoothie Served!" : "Drop files to upload"}
            </h3>
            <p className="mt-2 text-gray-500">
              {isBlending ? "Wait a moment, it's almost ready"
                : blendComplete ? "Your image is ready to use!"
                : <><span>or </span><span className="text-[#92A086] font-semibold hover:underline">browse</span> to choose a file</>}
            </p>
          </div>

          {/* Preview */}
          {blendComplete && previewUrl && (
            <div className="mt-5 p-3 bg-white rounded-lg shadow-md border border-[#92A086]/30">
              <p className="text-xs text-[#92A086] font-medium mb-2 text-center">📸 Your Image</p>
              <img src={previewUrl} alt="Uploaded preview" className="max-w-40 max-h-[100px] rounded-md object-cover mx-auto" />
              <button type="button" onClick={(e) => { e.stopPropagation(); resetUpload() }}
                className="mt-2 w-full py-1.5 px-3 text-xs font-medium text-[#7A8A70] bg-[#92A086]/10 hover:bg-[#92A086]/20 rounded-md transition-colors">
                Change image
              </button>
            </div>
          )}
        </div>
      </div>

      <style>{`
        @keyframes fruit-bounce { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-6px)} }
        @keyframes liquid-swirl { 0%,100%{transform:scaleX(1)} 50%{transform:scaleX(1.01)} }
        @keyframes glass-appear { 0%{opacity:0;transform:scale(0.9) translateY(10px)} 100%{opacity:1;transform:scale(1) translateY(0)} }
        .animate-fruit-bounce { animation: fruit-bounce 0.5s ease-in-out infinite; }
        .animate-liquid-swirl { animation: liquid-swirl 0.3s ease-in-out infinite; }
        .animate-glass-appear { animation: glass-appear 0.4s ease-out forwards; }
      `}</style>
    </div>
  )
}
```

**Props:**
```typescript
type BlenderUploadProps = {
  onFileSelect: (file: File, dataUrl: string) => void  // called when upload completes
  onError?: (message: string) => void   // validation/read errors
  accept?: string      // default: ".jpg,.jpeg,.png"
  maxSizeMB?: number   // default: 1
  disabled?: boolean
}
```

**Dependencies:**
```
React (hooks: useState, useCallback, useRef)
tailwindcss
Inline SVG only — no external icon libraries required
```

**Styles:**
Inline `<style>` tag with keyframe animations. Hardcoded brand accent `#92A086` (sage green) — replace with your own theme color via CSS variable or prop. Tailwind for layout/spacing.

**Usage:**
```tsx
<BlenderUpload
  onFileSelect={(file, dataUrl) => {
    setProfileImage(dataUrl)
    uploadToServer(file)
  }}
  onError={(msg) => alert(msg)}
  maxSizeMB={2}
  accept=".jpg,.jpeg,.png"
/>
```

**Prompt:**
> "Create a drag-and-drop file upload component with a playful blender animation. Show fruits falling in on drag. When uploading, animate liquid blending inside the jar. On completion transform to a smoothie glass with a cherry on top. Show a preview of the uploaded image below with a 'Change image' button. Use inline SVG — no external images."

**Registry JSON:**
```json
{
  "name": "BlenderUpload",
  "category": "forms",
  "tags": ["upload", "drag-drop", "animated", "file-input", "svg", "playful", "image-preview"]
}
```

## =========================
## === COMPONENT END ===
## =========================

---

## =========================
## === COMPONENT START ===
## =========================

**Name:** `EmptyState`

**Code:**
```tsx
import { Plus } from "lucide-react"

interface EmptyStateProps {
  title: string
  description: string
  icon?: React.ReactNode
  actionLabel?: string
  onAction?: () => void
  showAction?: boolean
}

export function EmptyState({
  title,
  description,
  icon,
  actionLabel = "Create",
  onAction,
  showAction = true,
}: EmptyStateProps) {
  return (
    <div className="flex flex-col items-center justify-center min-h-[400px] p-8 text-center">
      {icon && (
        <div className="mb-4 text-muted-foreground/40">
          {icon}
        </div>
      )}
      <h3 className="text-lg font-semibold mb-2">{title}</h3>
      <p className="text-sm text-muted-foreground max-w-md mb-6">{description}</p>
      {showAction && onAction && (
        <button
          onClick={onAction}
          className="inline-flex items-center gap-2 rounded-md bg-primary px-4 py-2 text-sm font-medium text-primary-foreground hover:bg-primary/90 transition-colors"
        >
          <Plus className="h-4 w-4" />
          {actionLabel}
        </button>
      )}
    </div>
  )
}
```

**Props:**
```typescript
type EmptyStateProps = {
  title: string
  description: string
  icon?: React.ReactNode     // any icon/illustration
  actionLabel?: string       // default: "Create"
  onAction?: () => void      // omit to hide button
  showAction?: boolean       // default: true
}
```

**Dependencies:**
```
lucide-react (Plus)
tailwindcss (primary, muted-foreground CSS vars)
```

**Styles:**
Tailwind utilities. Requires `--primary`, `--primary-foreground`, `--muted-foreground` tokens.

**Usage:**
```tsx
import { CalendarX } from "lucide-react"

<EmptyState
  icon={<CalendarX className="h-12 w-12" />}
  title="No events found"
  description="You haven't created any events yet. Get started by creating your first one."
  actionLabel="Create Event"
  onAction={() => setShowForm(true)}
/>
```

**Prompt:**
> "Create a centered empty state component with an optional icon slot, a heading, description text, and an optional primary CTA button with a plus icon. Use Tailwind CSS with theme tokens."

**Registry JSON:**
```json
{
  "name": "EmptyState",
  "category": "feedback",
  "tags": ["empty", "placeholder", "no-data", "cta", "illustration-slot"]
}
```

## =========================
## === COMPONENT END ===
## =========================

---

## =========================
## === COMPONENT START ===
## =========================

**Name:** `CheckboxVariants` (4 variants: `CustomCheckbox`, `GradientCheckbox`, `TransformerCheckbox`, `AnimatedCheckbox`)

**Code:**
```tsx
"use client"

import * as React from "react"
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

/** Ant Design-inspired blue checkbox with ripple effect */
const CustomCheckbox = React.forwardRef<HTMLInputElement, React.InputHTMLAttributes<HTMLInputElement>>(
  ({ className, ...props }, ref) => (
    <input type="checkbox" ref={ref}
      className={cn(
        "border-1 relative box-border block h-[1.5rem] w-[1.5rem] cursor-pointer appearance-none rounded-md border-[#d9d9d9] bg-slate-200 transition-all duration-300",
        "before:absolute before:left-2/4 before:top-[42%] before:h-[10px] before:w-[6px]",
        "before:-translate-x-2/4 before:-translate-y-2/4 before:rotate-45 before:scale-0",
        "before:border-b-2 before:border-r-2 before:border-solid before:border-b-white before:border-r-white",
        "before:opacity-0 before:transition-all before:delay-100 before:duration-100 before:ease-in before:content-['']",
        "after:absolute after:inset-0 after:rounded-[7px] after:opacity-0",
        "after:shadow-[0_0_0_calc(30px_/_2.5)_#1677ff] after:transition-all after:duration-500 after:ease-in after:content-['']",
        "checked:border-transparent checked:bg-[#1677ff]",
        "checked:before:scale-x-[1.4] checked:before:scale-y-[1.4] checked:before:opacity-100",
        "checked:before:transition-all checked:before:delay-100 checked:before:duration-200",
        "hover:border-[#1677ff] focus:outline-[#1677ff]",
        "[&:active:not(:checked)]:after:opacity-100 [&:active:not(:checked)]:after:shadow-none",
        className
      )}
      {...props}
    />
  )
)
CustomCheckbox.displayName = "CustomCheckbox"

/** Rainbow glow gradient checkbox */
const GradientCheckbox = React.forwardRef<HTMLInputElement, React.InputHTMLAttributes<HTMLInputElement>>(
  ({ className, ...props }, ref) => (
    <label className="relative block cursor-pointer select-none rounded-md text-3xl outline-2 outline-offset-1 outline-gray-700 has-[:focus-visible]:outline">
      <input ref={ref} type="checkbox" className="peer absolute opacity-0" {...props} />
      <div className={cn(
        "relative h-[1.6rem] w-[1.6rem] rounded-[0.3em] bg-white transition-all duration-300",
        "after:absolute after:left-0 after:top-0 after:h-[1.6rem] after:w-[1.6rem]",
        "after:rotate-0 after:rounded-[0.3em] after:border-[2px] after:border-[rgba(0,0,0,0.863)]",
        "after:transition-all after:delay-100 after:duration-300 after:content-['']",
        "peer-checked:bg-black",
        "peer-checked:shadow-[-13px_-13px_40px_0px_rgb(17,0,248),13px_0_40px_0px_rgb(243,11,243),13px_-13px_40px_0px_rgb(253,228,0),13px_0_40px_0px_rgb(107,255,21),13px_13px_40px_0px_rgb(76,0,255),-13px_13px_40px_0px_rgb(90,105,240)]",
        "peer-checked:after:left-2 peer-checked:after:top-[1px] peer-checked:after:h-[0.6em] peer-checked:after:w-[0.35em]",
        "peer-checked:after:rotate-45 peer-checked:after:rounded-[0em]",
        "peer-checked:after:border-b-[0.1em] peer-checked:after:border-r-[0.1em]",
        "peer-checked:after:border-[rgba(238,238,238,0)_white_white_#fff0]",
        "dark:bg-black dark:after:border-[rgba(255,255,255,0.863)]",
        "dark:peer-checked:bg-white dark:peer-checked:after:border-[rgba(238,238,238,0)_black_black_#fff0]",
        className
      )} />
    </label>
  )
)
GradientCheckbox.displayName = "GradientCheckbox"

/** Morphs into a checkmark on check */
const TransformerCheckbox = React.forwardRef<HTMLInputElement, React.InputHTMLAttributes<HTMLInputElement>>(
  ({ className, ...props }, ref) => (
    <label className="relative block h-[1.5rem] w-[1.5rem] cursor-pointer rounded-sm">
      <input ref={ref} type="checkbox" className="peer absolute h-0 w-0 opacity-0" {...props} />
      <span className={cn(
        "block h-[inherit] w-[inherit] rounded-md border-[2px] border-black transition-all duration-300",
        "peer-checked:ml-1 peer-checked:h-5 peer-checked:w-3",
        "peer-checked:translate-x-[2px] peer-checked:translate-y-[-1px] peer-checked:rotate-45",
        "peer-checked:rounded-none peer-checked:border-b-[2px] peer-checked:border-l-transparent peer-checked:border-t-transparent",
        "dark:border-white",
        className
      )} />
    </label>
  )
)
TransformerCheckbox.displayName = "TransformerCheckbox"

/** Round pulse checkbox in green */
const AnimatedCheckbox = React.forwardRef<HTMLInputElement, React.InputHTMLAttributes<HTMLInputElement>>(
  ({ className, ...props }, ref) => (
    <label className="relative block cursor-pointer select-none rounded-full text-2xl has-[:checked]:rounded-md">
      <input ref={ref} type="checkbox" className="peer absolute h-0 w-0 opacity-0" {...props} />
      <div className={cn(
        "relative h-[1.5rem] w-[1.5rem] rounded-[50%] bg-slate-200 transition duration-300",
        "after:absolute after:left-[0.5rem] after:top-1 after:hidden after:h-[0.8rem] after:w-[0.5rem]",
        "after:rotate-45 after:border-b-[0.2rem] after:border-r-[0.2rem] after:content-['']",
        "peer-checked:animate-pulse peer-checked:rounded-lg peer-checked:bg-[#0b6e4f] peer-checked:after:block",
        className
      )} />
    </label>
  )
)
AnimatedCheckbox.displayName = "AnimatedCheckbox"

export { CustomCheckbox, GradientCheckbox, TransformerCheckbox, AnimatedCheckbox }
```

**Props:**
All four extend `React.InputHTMLAttributes<HTMLInputElement>` — use as a standard `<input type="checkbox">`.

**Dependencies:**
```
clsx
tailwind-merge
tailwindcss (no custom tokens required — hardcoded colors)
```

**Styles:**
Pure Tailwind pseudo-class utilities. `GradientCheckbox` uses hardcoded RGB colors for rainbow glow. `AnimatedCheckbox` uses hardcoded `#0b6e4f` green.

**Usage:**
```tsx
// Blue ripple
<CustomCheckbox checked={isSelected} onChange={(e) => setIsSelected(e.target.checked)} />

// Rainbow glow (dark bg recommended)
<GradientCheckbox defaultChecked />

// Morphing checkmark
<TransformerCheckbox onChange={(e) => console.log(e.target.checked)} />

// Pulse green
<AnimatedCheckbox checked={active} onChange={(e) => setActive(e.target.checked)} />
```

**Prompt:**
> "Create four stylized checkbox variants using only Tailwind CSS and pseudo-elements — no SVGs. Include: (1) Ant Design style with blue ripple on click, (2) rainbow gradient glow on check, (3) border morphs into a checkmark stroke, (4) circular checkbox that pulses green when checked."

**Registry JSON:**
```json
{
  "name": "CheckboxVariants",
  "category": "forms",
  "tags": ["checkbox", "animated", "variants", "gradient", "morphing", "tailwind", "custom"]
}
```

## =========================
## === COMPONENT END ===
## =========================

---

## =========================
## === COMPONENT START ===
## =========================

**Name:** `LoadingSpinner`

**Code:**
```tsx
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

const sizes = {
  sm: "h-4 w-4 border-2",
  md: "h-8 w-8 border-3",
  lg: "h-12 w-12 border-4",
}

interface LoadingSpinnerProps {
  size?: "sm" | "md" | "lg"
  className?: string
}

export const LoadingSpinner = ({ size = "md", className }: LoadingSpinnerProps) => (
  <div className={cn("flex items-center justify-center", className)}>
    <div className={cn("animate-spin rounded-full border-primary border-t-transparent", sizes[size])} />
  </div>
)
```

**Props:**
```typescript
type LoadingSpinnerProps = {
  size?: "sm" | "md" | "lg"   // default: "md"
  className?: string            // wrapper div classes
}
```

**Dependencies:**
```
clsx
tailwind-merge
tailwindcss (--primary token, animate-spin)
```

**Styles:**
Tailwind spin animation + border trick. Requires `--primary` CSS variable.

**Usage:**
```tsx
<LoadingSpinner />
<LoadingSpinner size="lg" className="my-8" />
<LoadingSpinner size="sm" className="inline-block" />
```

**Prompt:**
> "Create a minimal centered loading spinner with three sizes (sm/md/lg) using Tailwind's animate-spin and a colored border with a transparent top to create the spinning arc effect."

**Registry JSON:**
```json
{
  "name": "LoadingSpinner",
  "category": "feedback",
  "tags": ["loading", "spinner", "animation", "minimal"]
}
```

## =========================
## === COMPONENT END ===
## =========================

---

## =========================
## === COMPONENT START ===
## =========================

**Name:** `PriceBreakdown`

**Code:**
```tsx
import { DivideIcon as LucideIcon } from "lucide-react"
// Actual icons used:
import { IndianRupee, Receipt, Percent } from "lucide-react"

interface PriceBreakdownProps {
  price: number
  taxPercent: number
  taxLabel?: string          // e.g. "GST", "VAT", "Tax"
  currencySymbol?: string    // default: "₹"
  currencyLocale?: string    // default: "en-IN"
  priceLabel?: string
}

export function PriceBreakdown({
  price,
  taxPercent,
  taxLabel = "Tax",
  currencySymbol = "₹",
  currencyLocale = "en-IN",
  priceLabel = "Base Price",
}: PriceBreakdownProps) {
  const validPrice = Number.isFinite(price) && price > 0 ? price : 0
  const validTax = Number.isFinite(taxPercent) && taxPercent >= 0 ? taxPercent : 0

  if (validPrice === 0) return null

  const taxAmount = validPrice * (validTax / 100)
  const totalPrice = validPrice + taxAmount

  const fmt = (n: number) =>
    `${currencySymbol}${n.toLocaleString(currencyLocale, { minimumFractionDigits: 2, maximumFractionDigits: 2 })}`

  return (
    <div className="rounded-lg border bg-muted/40 p-4 space-y-3">
      <div className="flex items-center gap-2 text-sm font-semibold text-muted-foreground uppercase tracking-wide">
        <Receipt className="h-4 w-4" />
        Price Breakdown
      </div>
      <div className="space-y-2">
        <div className="flex items-center justify-between text-sm">
          <span className="flex items-center gap-2 text-muted-foreground">
            <IndianRupee className="h-3.5 w-3.5" />
            {priceLabel}
          </span>
          <span className="font-medium">{fmt(validPrice)}</span>
        </div>
        <div className="flex items-center justify-between text-sm">
          <span className="flex items-center gap-2 text-muted-foreground">
            <Percent className="h-3.5 w-3.5" />
            {taxLabel} ({validTax}%)
          </span>
          <span className="font-medium">{taxAmount > 0 ? fmt(taxAmount) : "—"}</span>
        </div>
        <div className="border-t" />
        <div className="flex items-center justify-between">
          <span className="text-sm font-semibold">Total Price</span>
          <span className="text-lg font-bold text-primary">{fmt(totalPrice)}</span>
        </div>
      </div>
    </div>
  )
}
```

**Props:**
```typescript
type PriceBreakdownProps = {
  price: number           // Base price before tax
  taxPercent: number      // Tax rate 0–100
  priceLabel?: string     // default: "Base Price"
  taxLabel?: string       // default: "Tax"
  currencySymbol?: string // default: "₹"  → swap to "$", "€", etc.
  currencyLocale?: string // default: "en-IN" → swap to "en-US", "de-DE", etc.
}
```

**Dependencies:**
```
lucide-react (IndianRupee, Receipt, Percent)
tailwindcss (primary, muted, border CSS vars)
```

**Styles:**
Tailwind utilities. Requires `--primary`, `--muted`, `--border` CSS variable tokens. Renders `null` when `price === 0`.

**Usage:**
```tsx
// Indian GST scenario
<PriceBreakdown price={1000} taxPercent={18} taxLabel="GST" />

// USD + sales tax
<PriceBreakdown
  price={49.99}
  taxPercent={8.5}
  taxLabel="Sales Tax"
  currencySymbol="$"
  currencyLocale="en-US"
  priceLabel="Ticket Price"
/>
```

**Prompt:**
> "Create a read-only price breakdown card showing base price, tax amount, a divider, and total. Accept base price, tax percentage, currency symbol, locale string, and custom labels as props. Render nothing when price is zero or invalid. Use lucide-react icons and Tailwind CSS."

**Registry JSON:**
```json
{
  "name": "PriceBreakdown",
  "category": "cards",
  "tags": ["pricing", "tax", "breakdown", "receipt", "finance", "display"]
}
```

## =========================
## === COMPONENT END ===
## =========================

---

## =========================
## === COMPONENT START ===
## =========================

**Name:** `Pagination`

**Code:**
```tsx
import * as React from "react"
import { ChevronLeft, ChevronRight, MoreHorizontal } from "lucide-react"
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"
import { cva } from "class-variance-authority"

function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

const buttonVariants = cva(
  "inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50",
  {
    variants: {
      variant: {
        outline: "border border-input bg-background hover:bg-accent hover:text-accent-foreground",
        ghost: "hover:bg-accent hover:text-accent-foreground",
      },
      size: {
        default: "h-10 px-4 py-2",
        icon: "h-10 w-10",
      },
    },
    defaultVariants: { variant: "ghost", size: "icon" },
  }
)

const Pagination = ({ className, ...props }: React.ComponentProps<"nav">) => (
  <nav role="navigation" aria-label="pagination"
    className={cn("mx-auto flex w-full justify-center", className)} {...props} />
)

const PaginationContent = React.forwardRef<HTMLUListElement, React.ComponentProps<"ul">>(
  ({ className, ...props }, ref) => (
    <ul ref={ref} className={cn("flex flex-row items-center gap-1", className)} {...props} />
  )
)

const PaginationItem = React.forwardRef<HTMLLIElement, React.ComponentProps<"li">>(
  ({ className, ...props }, ref) => <li ref={ref} className={cn("", className)} {...props} />
)

type PaginationLinkProps = { isActive?: boolean } & React.ComponentProps<"a">

const PaginationLink = ({ className, isActive, ...props }: PaginationLinkProps) => (
  <a aria-current={isActive ? "page" : undefined}
    className={cn(buttonVariants({ variant: isActive ? "outline" : "ghost", size: "icon" }), className)}
    {...props} />
)

const PaginationPrevious = ({ className, ...props }: React.ComponentProps<typeof PaginationLink>) => (
  <PaginationLink aria-label="Go to previous page"
    className={cn("gap-1 pl-2.5 w-auto px-3", className)} {...props}>
    <ChevronLeft className="h-4 w-4" />
    <span>Previous</span>
  </PaginationLink>
)

const PaginationNext = ({ className, ...props }: React.ComponentProps<typeof PaginationLink>) => (
  <PaginationLink aria-label="Go to next page"
    className={cn("gap-1 pr-2.5 w-auto px-3", className)} {...props}>
    <span>Next</span>
    <ChevronRight className="h-4 w-4" />
  </PaginationLink>
)

const PaginationEllipsis = ({ className, ...props }: React.ComponentProps<"span">) => (
  <span aria-hidden className={cn("flex h-10 w-10 items-center justify-center", className)} {...props}>
    <MoreHorizontal className="h-4 w-4" />
    <span className="sr-only">More pages</span>
  </span>
)

export {
  Pagination, PaginationContent, PaginationItem,
  PaginationLink, PaginationNext, PaginationPrevious, PaginationEllipsis,
}
```

**Props:**
Composable primitives — all extend their underlying HTML element props. `PaginationLink` adds `isActive?: boolean`.

**Dependencies:**
```
lucide-react (ChevronLeft, ChevronRight, MoreHorizontal)
class-variance-authority
clsx
tailwind-merge
tailwindcss (ring, accent, background, input CSS vars)
```

**Styles:**
Fully Tailwind-based. Requires `--ring`, `--accent`, `--background`, `--input` tokens.

**Usage:**
```tsx
<Pagination>
  <PaginationContent>
    <PaginationItem>
      <PaginationPrevious href="/posts?page=2" />
    </PaginationItem>
    {[1,2,3].map(page => (
      <PaginationItem key={page}>
        <PaginationLink href={`/posts?page=${page}`} isActive={page === 3}>
          {page}
        </PaginationLink>
      </PaginationItem>
    ))}
    <PaginationItem><PaginationEllipsis /></PaginationItem>
    <PaginationItem>
      <PaginationNext href="/posts?page=4" />
    </PaginationItem>
  </PaginationContent>
</Pagination>
```

**Prompt:**
> "Create an accessible, composable pagination component using shadcn/ui patterns. Include Pagination, PaginationContent, PaginationItem, PaginationLink (with isActive), PaginationPrevious, PaginationNext, and PaginationEllipsis. Use lucide-react for chevron and ellipsis icons. Keep it fully keyboard and screen-reader accessible."

**Registry JSON:**
```json
{
  "name": "Pagination",
  "category": "navigation",
  "tags": ["pagination", "composable", "accessible", "aria", "shadcn"]
}
```

## =========================
## === COMPONENT END ===
## =========================

---

## Summary Table

| # | Component | Category | Key deps | Unique trait |
|---|-----------|----------|----------|--------------|
| 1 | `TubelightNavBar` | navigation | framer-motion, lucide-react | Spring-animated lamp glow on active tab |
| 2 | `MD3Switch` | forms | cva, lucide-react | MD3 physics, haptic audio, halo hover |
| 3 | `DualConfirmDialog` | dialogs | shadcn/ui, lucide-react | 2-step destructive confirm + progress bar |
| 4 | `BlenderUpload` | forms | React only (inline SVG) | Blender animation → smoothie glass on complete |
| 5 | `EmptyState` | feedback | lucide-react | Slot-based icon, optional CTA |
| 6 | `CheckboxVariants` | forms | clsx, tailwind-merge | 4 animated variants, pure Tailwind |
| 7 | `LoadingSpinner` | feedback | clsx | 3-size border-spin spinner |
| 8 | `PriceBreakdown` | cards | lucide-react | Tax calc display, locale-aware currency |
| 9 | `Pagination` | navigation | cva, lucide-react | Accessible composable pagination |

**Shared utility required by all components:**
```ts
// utils.ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"
export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```
Install: `npm install clsx tailwind-merge` 

