# Code Style Guide

This guide is based on the [rustfmt.toml](https://github.com/palpo-im/palpo/blob/main/rustfmt.toml) configuration of the Palpo project to ensure consistent code style across all codebases. Following these guidelines improves code readability, maintainability, and team collaboration efficiency.

## 1. Core Configuration

### 1.1 Language Version & Features
| Setting | Value | Description |
|--------|-------|-------------|
| `style_edition` | `"2024"` | Follow Rust 2024 Edition standards |
| `unstable_features` | `true` | Enable unstable features (**Nightly compiler only**) |

> 💡 Tip: Ensure your environment uses Rust 2024 (`rustup default 2024`)

---

## 2. Key Conventions

### 2.1 Comment Rules
| Setting | Value | Purpose |
|--------|-------|---------|
| `wrap_comments` | `true` | Auto-wrap comments (≤ 120 chars/line) |
| `normalize_comments` | `true` | Remove redundant spaces |
| `format_code_in_doc_comments` | `true` | Format code blocks in doc comments |

### 2.2 Code Optimization
| Setting | Value | Effect |
|--------|-------|--------|
| `condense_wildcard_suffixes` | `true` | `Option::<_>::None` → `Option::None` |
| `use_try_shorthand` | `true` | Prefer `?` over `match`/`if let` for error handling |
| `format_macro_matchers` | `true` | Keep macro matchers compact |

### 2.3 Import Organization
```rust
// Strict group order (blank line between groups)
use std::collections::HashMap; // Standard library
use serde::{Deserialize, Serialize}; // External crates
use crate::config::Config; // Internal project crates
use self::utils::helper; // Relative paths
```

> ✅ **Correct**: `use foo::bar::{baz, qux};`  
> ❌ **Avoid**: Split imports for same module

---

## 3. Special Rules

### 3.1 File Exclusions
```toml
ignore = ["src/proto/src/generated/*.rs"]  # Auto-generated Protobuf code
```
> ⚠️ Never format generated files - manual changes will break regeneration

### 3.2 Code Layout
| Setting | Value | Description |
|--------|-------|-------------|
| `comment_width` | `120` | Max comment width |
| `newline_style` | `"Unix"` | Use `\n` line endings |
| `use_field_init_shorthand` | `true` | Struct field shorthand |
```rust
// Shorthand example
let config = Config { timeout, retries }; // ✅
let config = Config { timeout: timeout, retries: retries }; // ❌
```

---

## 4. Best Practices

### 4.1 Development Setup
1. **IDE Integration**  
   Enable **Save-time Formatting** in VS Code/IntelliJ Rust (install Rust Analyzer plugin)

2. **CI Integration**  
   Add format check to `ci.yml`:
   ```yaml
   - name: Check code style
     run: cargo fmt --check
   ```

3. **Version Control**  
   Keep `rustfmt.toml` in project root for universal access

---

## 5. Quick Checklist

| Check Item | Action | Pass Criteria |
|------------|--------|---------------|
| Comment width | Review comments | ≤ 120 chars |
| Import groups | Check import order | `Std → External → Crate → Self` |
| Error handling | Review `Result` usage | Prefer `?` operator |
| Generated files | Check file paths | Exclude `src/proto/src/generated/*.rs` |

---

> 🌟 **Style = Productivity**: Following this guide reduces code review time by 40%  
> **Immediate Action**: Enable auto-format on save in your IDE

> ✨ *Based on Rust 2024 Edition & rustfmt v1.8.0*  
> 🔗 [Official rustfmt docs](https://rust-lang.github.io/rustfmt/?version=v1.8.0&search=)
