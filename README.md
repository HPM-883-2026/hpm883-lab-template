# HPM 883 Lab Template

A reproducible analysis template for HPM 883: Advanced Quantitative Methods.

## Quick Start

### 1. Clone this repository
```bash
git clone <your-assignment-url>
cd <your-repo-name>
```

### 2. Open in Positron (or RStudio)
- Open the `.Rproj` file, OR
- Open the folder in Positron

### 3. Restore the R environment
```r
install.packages("renv")  # Only needed once
renv::restore()
```

### 4. Complete your analysis
- Edit `analysis.qmd` to complete the lab
- Render to check your work: Click "Render" or run `quarto render analysis.qmd`

### 5. Submit
```bash
git add .
git commit -m "Complete Lab X"
git push
```

---

## Repository Structure

```
hpm883-lab-template/
├── README.md           # This file - instructions and rubric
├── .gitignore          # Files Git should ignore
├── .Rprofile           # Automatically loads renv
├── renv.lock           # Package versions (reproducibility)
├── renv/               # renv infrastructure
├── *.Rproj             # RStudio/Positron project file
├── data/
│   └── raw/            # Input data (DO NOT MODIFY)
├── analysis.qmd        # YOUR WORK GOES HERE
└── output/             # Generated results
```

---

## Best Practices

1. **Never modify files in `data/raw/`** - These are your original inputs
2. **Commit often** - Save your work with meaningful commit messages
3. **Render before submitting** - Make sure your document compiles
4. **Use the AI assistant responsibly** - Positron's AI can help, but you must understand the code

---

## Troubleshooting

### "Package not found" error
```r
renv::restore()  # Reinstall all packages
```

### Render fails
- Check for typos in your R code
- Make sure all code chunks run without errors
- Try running each chunk individually first

### Git issues
```bash
git status              # See what's changed
git diff                # See specific changes
git log --oneline -5    # See recent commits
```

---

## Getting Help

- **Slack**: Post questions in #hpm883-help
- **Office Hours**: See syllabus for times
- **AI Assistant**: Use Positron's built-in AI for coding help

---

*HPM 883: Advanced Quantitative Methods | Spring 2026*
