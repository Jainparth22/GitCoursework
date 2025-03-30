# Module 27: Monorepos & Scaling Git

> **Level**: ⚫ Professional | **Time**: 3 hours | **Prerequisites**: [Module 26](../26-Open-Source-Contribution-Guide/README.md)

---

## 📋 Learning Objectives

- Understand monorepo architecture and trade-offs
- Use sparse checkout and partial clone for large repos
- Optimize Git performance at scale

---

## 1. Monorepo Architecture

```mermaid
graph TD
    subgraph "Monorepo"
        ROOT["packages/"]
        ROOT --> PKG_A["frontend/<br/>(React app)"]
        ROOT --> PKG_B["backend/<br/>(Node API)"]
        ROOT --> PKG_C["shared/<br/>(common utils)"]
        ROOT --> PKG_D["mobile/<br/>(React Native)"]
        
        SHARED_CFG["Root Config<br/>package.json<br/>tsconfig.json<br/>.eslintrc"]
    end
    
    style ROOT fill:#74c0fc
    style PKG_A fill:#51cf66
    style PKG_B fill:#51cf66
    style PKG_C fill:#ffd43b
    style PKG_D fill:#51cf66
```

### Monorepo vs Polyrepo

| Factor | Monorepo | Polyrepo |
|--------|----------|----------|
| Code sharing | Easy (same repo) | Requires publishing packages |
| Atomic changes | Cross-package in one PR ✅ | Multiple PRs across repos |
| CI complexity | Must detect what changed | Simpler per-repo CI |
| Repo size | Can become very large | Stays small |
| Onboarding | All code visible | Focused per service |

---

## 2. Sparse Checkout — Work on a Subset

```mermaid
flowchart TD
    A["Huge monorepo<br/>(100GB, 50 packages)"] --> B["Sparse Checkout"]
    B --> C["Only checkout<br/>frontend/ and shared/"]
    C --> D["Local clone: ~2GB<br/>(just what you need)"]
    
    style A fill:#ff6b6b
    style D fill:#51cf66
```

```bash
# Clone without checking out files
git clone --no-checkout https://github.com/org/monorepo.git
cd monorepo

# Enable sparse checkout
git sparse-checkout init --cone

# Only checkout these directories
git sparse-checkout set frontend shared

# Add more directories later
git sparse-checkout add backend

# View current sparse-checkout config
git sparse-checkout list
```

---

## 3. Partial Clone — Download Less Data

```mermaid
flowchart TD
    FULL["Full Clone<br/>All commits + all blobs<br/>100GB"] 
    BLOBLESS["Blobless Clone<br/>All commits, blobs on demand<br/>5GB"]
    TREELESS["Treeless Clone<br/>Only reachable trees + blobs<br/>1GB"]
    
    style FULL fill:#ff6b6b
    style BLOBLESS fill:#ffd43b
    style TREELESS fill:#51cf66
```

```bash
# Blobless clone (recommended)
git clone --filter=blob:none URL

# Treeless clone (smallest, but slower operations)
git clone --filter=tree:0 URL

# Shallow clone (specific depth)
git clone --depth=1 URL
```

---

## 4. Performance Optimization

```mermaid
graph TD
    PERF["Git Performance at Scale"]
    PERF --> FSM["git maintenance<br/>(background optimization)"]
    PERF --> COM["Commit graph<br/>(faster log, merge-base)"]
    PERF --> MID["Multi-pack index<br/>(faster object lookup)"]
    PERF --> SC["Sparse checkout<br/>(less on disk)"]
    
    style FSM fill:#51cf66
    style COM fill:#74c0fc
```

```bash
# Enable background maintenance
git maintenance start

# Force garbage collection
git gc --aggressive

# Update commit-graph for faster log
git commit-graph write --reachable

# Check repo health
git fsck
git count-objects -v
```

---

## 🏋️ Exercises

1. Create a monorepo structure with 3 packages and shared config
2. Use sparse checkout to only clone 1 package from a large repo
3. Compare clone times: full vs blobless vs shallow
4. Enable `git maintenance` and observe background optimization

---

## 🔑 Key Takeaways

1. Monorepos centralize code but require tooling for scale
2. **Sparse checkout** limits which directories are on disk
3. **Partial clone** downloads object data on demand
4. `git maintenance` automates background performance optimization
5. Choose monorepo vs polyrepo based on team size and coupling

---

**[← Module 26](../26-Open-Source-Contribution-Guide/README.md)** | **[Module 28 →](../28-Repository-Best-Practices/README.md)**
