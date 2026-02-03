# Ori: Offline-First Learning Platform

<div align="center">


**Learn offline, sync when connected**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Rust](https://img.shields.io/badge/rust-1.75%2B-orange.svg)](https://www.rust-lang.org)
[![Node](https://img.shields.io/badge/node-18%2B-green.svg)](https://nodejs.org)

[Features](#-features) • [Quick Start](#-quick-start) • [Documentation](#-documentation) • [Roadmap](#-roadmap) • [Contributing](#-contributing)

</div>

---

##  What is Ori?

**Ori** (Yoruba for "head/mind") is an open-source, offline-first educational platform designed for students in resource-constrained environments. Built with Rust and Svelte, Ori enables learning for weeks without internet connectivity, syncing progress and content only when connected.

### The Problem

Millions of students in rural areas face:
-  Unreliable or no internet connectivity
-  Low-end devices (2GB RAM or less)
-  Limited power supply
-  Expensive mobile data
-  Lack of aligned curriculum content

### The Solution

Ori provides:
-  **Severe Offline-First**: Full functionality for weeks without internet
-  **Curriculum-Aligned**: WAEC and NECO exam preparation content, Tech Curriculums...
-  **Localized**: English, Hausa, Yoruba, and Igbo languages
-  **Accessible**: WCAG AAA compliance for all users
-  **Performant**: Optimized for low-end devices

---

##  Features

### Core Functionality

- ** Offline Content**: Download entire courses as ZIP bundles, study for weeks without connectivity
- ** Progress Tracking**: Local progress saved instantly, syncs automatically when online
- ** Quiz System**: Take assessments offline, results sync when connected
- ** Video Player**: Resume from where you left off, even after days offline
- ** PDF Viewer**: Study documents with progress tracking
- ** Conflict-Free Sync**: CRDTs ensure no data loss during multi-device sync

### Educational Features

- ** Course Library**: WAEC/NECO-aligned Mathematics, Chemistry, English, and more
- ** Structured Learning**: Lessons organized with prerequisites and learning paths
- ** Interactive Quizzes**: Multiple choice, true/false, and short answer questions
- ** Certificates**: Generate completion certificates locally. Cyptographically signed.
- ** Analytics**: Track time spent, lessons completed, and quiz performance

### Technical Features

- ** Rust Backend**: Memory-safe, concurrent, high-performance core
- ** Svelte Frontend**: Lightweight, reactive UI framework
- ** Local-First Storage**: SQLite + Sled key-value store + IndexedDB
- ** Conflict Resolution**: Loro CRDTs for automatic merge without data loss
- ** Progressive Web App**: Install on any device, works like a native app
- ** Cross-Platform**: Web, Windows, macOS, Linux, Android (iOS planned)

### School Mode

- ** LAN Distribution**: Share content across school network without internet
- ** Teacher Dashboard**: Monitor student progress, manage classes
- ** mDNS Discovery**: Automatic server discovery on local network
- ** USB Import/Export**: Transfer content via USB drives
- ** Multi-User Support**: Multiple students can use the same device

---

## Quick Start

### Prerequisites

- **Rust** 1.75+ ([Install](https://rustup.rs))
- **Node.js** 18+ ([Install](https://nodejs.org))
- **Git**

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/ori.git
cd ori

# Install Rust dependencies
cd ori-core
cargo build --release

# Install Frontend dependencies
cd ../ori-web
npm install

# Run development servers
npm run dev
```

Visit `http://localhost:5173` to see Ori in action!

### Quick Demo

```bash
# Try the design system demo
cd ori-design-system
open DEMO.html  # or double-click the file
```

---

## Documentation

### For Users

- **[User Guide](docs/user-guide.md)** - How to use Ori
- **[Installation Guide](docs/installation.md)** - Detailed setup instructions
- **[FAQ](docs/faq.md)** - Common questions

### For Developers

- **[Architecture](docs/architecture.md)** - System design and technical overview
- **[Contributing Guide](CONTRIBUTING.md)** - How to contribute
- **[Development Setup](docs/dev-setup.md)** - Local development environment
- **[API Reference](docs/api-reference.md)** - Rust and Svelte APIs
- **[Design System](ori-design-system/README.md)** - UI components and guidelines

### For Content Creators

- **[Content Creation Guide](docs/content-creation.md)** - How to create course bundles
- **[Bundle Format](docs/bundle-format.md)** - Technical specification
- **[Content Guidelines](docs/content-guidelines.md)** - Best practices

---

## Roadmap

### Current Status: Phase 1 (Rust Core MVP)

- [x] **Phase 0: Foundation**  (Completed Feb 2026)
  - Design system complete
  - Architecture documented
  - Repository structure defined

- [ ] **Phase 1: Rust Core MVP** (In Progress)
  - Content manager (import/export bundles)
  - SQLite database schema
  - CRDT integration (Loro)
  - Progress tracking
  - Sync queue

- [ ] **Phase 2: Frontend Basics** (Next)
  - SvelteKit setup with design system
  - Course catalog and lesson viewer
  - Progress dashboard
  - Basic offline support

### Future Phases

- **Phase 3**: Offline & Sync (Complete PWA functionality)
- **Phase 4**: Learning Features (Quizzes, videos, interactives)
- **Phase 5**: School Mode (LAN server, multi-user)
- **Phase 6**: Cross-Platform (Desktop and mobile apps)
- **Phase 7**: Content & Localization (Nigerian curriculum, translations)
- **Phase 8**: Polish & v1.0 Release
- **Phase 9**: Public Launch 

See [detailed milestones](https://github.com/yourusername/ori/milestones) for full roadmap.

---

## Architecture

### High-Level Overview

```
┌─────────────────────────────────────────┐
│     Svelte Frontend (SvelteKit)         │
│  • PWA with Service Worker              │
│                                         │
│  • Offline-first UI                     │
└──────────────┬──────────────────────────┘
               │ (WASM/FFI)
┌──────────────▼──────────────────────────┐
│         Rust Core (WASM/Native)         │
│  • Content Manager                      │
│  • Progress Tracker (CRDTs)             │
│  • Sync Engine                          │
│  • Search Index (Tantivy)               │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│         Storage Layer                   │
│  • SQLite (Metadata)                    │
│  • Sled (CRDT state)                    │
│  • File System (Content bundles)        │
└─────────────────────────────────────────┘
```

### Tech Stack

**Backend:**
- **Rust** - Core logic, memory safety, concurrency
- **Tokio** - Async runtime
- **SQLite** (rusqlite) - Relational data
- **Sled** - Embedded key-value store
- **Loro** - CRDTs for conflict-free sync
- **Axum** - Local HTTP server (school mode)
- **Tantivy** - Full-text search

**Frontend:**
- **Svelte/SvelteKit** - Reactive UI framework
- **TypeScript** - Type safety
- **WASM** (wasm-bindgen) - Rust ↔ JavaScript bridge
- **Service Worker** - Offline caching
- **IndexedDB** - Browser storage

**Cross-Platform:**
- **Tauri** - Desktop apps (Windows, macOS, Linux)
- **Tauri Mobile** - Android/iOS apps
- **PWA** - Web apps (installable)

---

## 🤝 Contributing

We welcome contributions from developers, designers, educators, and students!

### How to Contribute

1. **🐛 Report Bugs** - [Open an issue](https://github.com/yourusername/ori/issues/new?template=bug_report.md)
2. **💡 Suggest Features** - [Open a discussion](https://github.com/yourusername/ori/discussions/new)
3. **📝 Improve Docs** - Documentation PRs always welcome
4. **🎨 Design** - UI/UX improvements, icons, illustrations
5. **📚 Create Content** - Contribute course bundles
6. **🌍 Translate** - Help localize to more languages
7. **💻 Code** - Pick an issue and submit a PR

### Good First Issues

New to the project? Look for issues labeled [`good-first-issue`](https://github.com/yourusername/ori/labels/good-first-issue).

### Development Workflow

```bash
# 1. Fork and clone
git clone https://github.com/YOUR_USERNAME/ori.git

# 2. Create a branch
git checkout -b feat/my-amazing-feature

# 3. Make changes and test
cargo test  # Rust tests
npm test    # Frontend tests

# 4. Commit (use conventional commits)
git commit -m "feat: add amazing feature"

# 5. Push and create PR
git push origin feat/my-amazing-feature
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

---

## 📄 License

Ori is dual-licensed under:

- **MIT License** ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)
- **Apache License 2.0** ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)

You may choose either license for your use.

### Why Dual License?

- **Flexibility**: Users can choose the license that works best for their project
- **Compatibility**: Works with both MIT and Apache ecosystems
- **Patent Protection**: Apache 2.0 provides explicit patent grants
- **Open Source Friendly**: Both are OSI-approved

---

## 🙏 Acknowledgments

### Inspiration

- **Kolibri** - Offline learning platform inspiration
- **KA Lite** - Proof of offline education viability
- **Swiss Design** - Helvetica, grid systems, minimalism
- **African Fractals** - Mathematical patterns in African design

### Built With

- **Rust** community and ecosystem
- **Svelte** team and contributors
- **Open Educational Resources** (OER) movement
- **WCAG** accessibility guidelines

### Special Thanks

- Nigerian teachers and students who inspired this project
- Open source contributors worldwide
- Everyone working to make education accessible

---

## Contact & Support

- **Issues**: [GitHub Issues](https://github.com/yourusername/ori/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/ori/discussions)
- **Email**: hello@ori.app (if you set this up)
- **Twitter**: [@OriLearn](https://twitter.com/OriLearn) (if you create this)

---

## Star History

If you find Ori useful, please consider starring the repository! ⭐

---

<div align="center">

**Built with ❤️ for EVERYONE **

*Empowering education through offline-first technology*

[⬆ Back to Top](#ori---offline-first-learning-platform)

</div>
