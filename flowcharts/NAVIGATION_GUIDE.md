# Flowchart Navigation Guide

## 🗺️ Quick Visual Map

```
flowcharts/
│
├── 📍 1-current-system/     ← "Show me what exists today"
│   └── Single practitioner, $89/$69 pricing
│
├── 🚀 2-new-update/         ← "Show me what we're building"
│   └── Multi-practitioner, free triage, $119/$79
│
├── 💡 3-future-options/     ← "Show me future possibilities"  
│   └── Service expansions (weight loss, counseling, etc.)
│
└── 📊 _analysis/            ← "Show me comparisons & reports"
    └── Pain points, improvements, decision matrices
```

## 🎯 Common Navigation Scenarios

### "I want to understand the current booking process"
→ Go to `1-current-system/booking/`

### "I want to see how the new triage system works"
→ Go to `2-new-update/consultant/triage-flow.md`

### "I want to compare current vs new admin features"
→ Compare:
- `1-current-system/admin/` (current)
- `2-new-update/admin/` (new)

### "I want to see future service expansion options"
→ Go to `3-future-options/`

### "I want to understand what's changing technically"
→ Compare:
- `1-current-system/tech/` (monolithic)
- `2-new-update/tech/` (microservices)

## 📁 Folder Purpose Summary

| Folder | Purpose | Key Question |
|--------|---------|--------------|
| **1-current-system** | Document existing system | "What do we have now?" |
| **2-new-update** | Implementation specs | "What are we building?" |
| **3-future-options** | Strategic planning | "What could we build later?" |
| **_analysis** | Decision support | "How do they compare?" |

## 🔄 Typical User Journeys

### Developer Journey
1. Start at `1-current-system/tech/` to understand current architecture
2. Move to `2-new-update/tech/` to see required changes
3. Check `docs/TECHNICAL_SPECIFICATIONS.md` for implementation details

### Business Stakeholder Journey
1. Start at `_analysis/improvements-summary.md` to understand why changes
2. Review `2-new-update/booking/` to see new patient experience
3. Explore `3-future-options/` for growth opportunities

### New Team Member Journey
1. Start with this guide
2. Review `1-current-system/README.md` to understand today
3. Review `2-new-update/README.md` to understand tomorrow
4. Browse specific roles as needed

## 💡 Navigation Tips

- **Each major folder has a README** - Start there for context
- **Numbered folders show progression** - 1→2→3 represents timeline
- **Underscored folders are meta** - _analysis sits outside the timeline
- **Use comparisons** - Most value comes from comparing same files across versions

---

📚 Back to [Main README](../README.md)