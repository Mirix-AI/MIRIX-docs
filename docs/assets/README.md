# Assets Directory

This directory contains images, logos, and other media files used in the documentation.

## Required Files

- `logo.png` - MIRIX logo (referenced in main documentation)

## Adding Images

To add images to your documentation:

1. Place image files in this directory
2. Reference them in markdown with relative paths:
   ```markdown
   ![Alt text](assets/image-name.png)
   ```

## Logo Specifications

For the main MIRIX logo:
- Format: PNG with transparency
- Recommended size: 200x200 pixels
- Background: Transparent
- Style: Should represent AI/memory/intelligence themes

## File Organization

```
docs/assets/
├── logo.png           # Main MIRIX logo
├── screenshots/       # Application screenshots
├── diagrams/         # Architecture diagrams (if not using mermaid)
└── icons/            # Small icons and graphics
```

## Note

Currently, `logo.png` is referenced in the documentation but not present. You should add your MIRIX logo here or update the references in the documentation files to remove the logo image. 