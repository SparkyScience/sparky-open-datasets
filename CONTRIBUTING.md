# Contributing to Sparky Open Datasets

Thank you for your interest in contributing to Sparky Open Datasets! This document provides guidelines for contributing to this repository.

## 🤝 Ways to Contribute

### 1. Report Issues
- **Data Quality Issues:** Found errors, inconsistencies, or missing data
- **Documentation Issues:** Unclear instructions, typos, or outdated information
- **Bugs:** Problems with scripts or data processing tools
- **Feature Requests:** Suggestions for new datasets or improvements

**How to report:** [Create an issue](https://github.com/SparkyScience/sparky-open-datasets/issues/new) with a clear title and detailed description.

### 2. Improve Documentation
- Fix typos or grammatical errors
- Clarify existing documentation
- Add usage examples
- Translate documentation
- Improve data dictionaries

### 3. Enhance Data Quality
- Identify and report data quality issues
- Suggest data cleaning improvements
- Propose data validation checks
- Contribute data enrichment scripts

### 4. Submit New Datasets
We welcome contributions of high-quality datasets that align with our mission. See Dataset Contribution Guidelines below.

### 5. Improve Code and Scripts
- Optimize data processing scripts
- Add new analysis examples
- Improve error handling
- Add tests

## 📋 Contribution Process

### For Small Changes (Documentation, Typos, etc.)
1. Fork the repository
2. Create a branch: `git checkout -b fix/your-fix-name`
3. Make your changes
4. Commit: `git commit -m "Brief description of changes"`
5. Push: `git push origin fix/your-fix-name`
6. Open a Pull Request

### For Larger Changes (New Features, Datasets)
1. **First, open an issue** to discuss your proposed changes
2. Wait for feedback and approval from maintainers
3. Fork the repository
4. Create a branch: `git checkout -b feature/your-feature-name`
5. Implement your changes
6. Add tests if applicable
7. Update documentation
8. Commit with clear messages
9. Push and open a Pull Request

## 📊 Dataset Contribution Guidelines

### Dataset Requirements

To be accepted, a dataset must:

1. **Quality**
   - Be clean, validated, and well-structured
   - Have no sensitive personal information
   - Be free of copyright issues

2. **Documentation**
   - Include a comprehensive README.md
   - Provide a detailed data dictionary
   - Specify collection methodology
   - Include license information

3. **Format**
   - Provide data in at least 2 formats (JSON, CSV, or Parquet recommended)
   - Use UTF-8 encoding
   - Follow consistent naming conventions

4. **Legal and Ethical**
   - Comply with applicable laws (GDPR, CCPA, etc.)
   - Include data collection disclosure
   - Specify usage restrictions if any
   - Provide proper attribution

5. **Size**
   - Datasets under 100 MB: Include directly in repository
   - Datasets 100 MB - 1 GB: Consider Git LFS
   - Datasets over 1 GB: Host externally and provide download links

### Dataset Submission Process

1. **Prepare Your Dataset**
   - Clean and validate the data
   - Export in multiple formats
   - Generate summary statistics
   - Create data dictionary

2. **Create Documentation**
   - Use the [Dataset README Template](docs/templates/DATASET_README_TEMPLATE.md)
   - Write comprehensive data dictionary
   - Document collection methodology
   - Include usage examples

3. **Open an Issue**
   - Title: "New Dataset: [Your Dataset Name]"
   - Describe the dataset, source, and value
   - Link to external preview if available
   - Wait for maintainer feedback

4. **Submit Pull Request**
   - Create folder: `datasets/your-dataset-name/`
   - Include all required files (see structure below)
   - Reference the issue in your PR description

### Required Dataset Files

```
datasets/your-dataset-name/
├── README.md                    # Comprehensive documentation
├── data_dictionary.md           # Field definitions
├── your_dataset.json           # Data in JSON format
├── your_dataset.csv            # Data in CSV format
├── your_dataset.parquet        # (Optional) Data in Parquet format
├── LICENSE                     # Dataset-specific license (if different)
└── examples/                   # (Optional) Usage examples
    └── analysis_example.py
```

## 💻 Code Style Guidelines

### Python
- Follow PEP 8 style guide
- Use meaningful variable names
- Add docstrings to functions
- Include type hints where appropriate
- Keep functions focused and modular

```python
def clean_data(raw_data: list[dict]) -> list[dict]:
    """
    Clean raw data by removing duplicates and standardizing fields.

    Args:
        raw_data: List of raw data records

    Returns:
        List of cleaned data records
    """
    # Implementation
    pass
```

### Documentation
- Use clear, concise language
- Provide examples
- Use proper Markdown formatting
- Include code blocks with syntax highlighting

## 🧪 Testing

If contributing code:
- Write unit tests for new functions
- Ensure existing tests pass
- Add integration tests for data processing pipelines
- Test with sample data

## 📝 Commit Message Guidelines

Use clear, descriptive commit messages:

```
<type>: <short summary>

<optional detailed description>

<optional footer>
```

**Types:**
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation changes
- `style:` Formatting changes
- `refactor:` Code refactoring
- `test:` Adding tests
- `chore:` Maintenance tasks

**Examples:**
```
feat: add GenAI jobs dataset with 5000+ postings

docs: improve ESG dataset README with usage examples

fix: correct timestamp parsing in data processing script
```

## 🔍 Review Process

1. **Automated Checks**
   - CI/CD pipeline runs automated tests
   - Linting and formatting checks
   - File size validation

2. **Manual Review**
   - Maintainer reviews code/documentation
   - Checks compliance with guidelines
   - Verifies data quality

3. **Feedback**
   - Reviewers may request changes
   - Address feedback and update PR
   - Discussion happens in PR comments

4. **Approval and Merge**
   - Once approved, PR will be merged
   - You'll be credited as a contributor

## ⚖️ Legal and Ethical Guidelines

### Data Collection
- Always respect robots.txt and terms of service
- Use ethical web scraping practices
- Implement rate limiting
- Do not collect personal information without consent

### Privacy
- Remove all personally identifiable information (PII)
- Anonymize data when necessary
- Comply with GDPR, CCPA, and other regulations
- Document any privacy considerations

### Licensing
- Ensure you have rights to share the data
- Specify appropriate license (recommend CC BY 4.0)
- Respect third-party intellectual property
- Provide proper attribution

## 🏆 Recognition

Contributors will be:
- Listed in the repository's contributors page
- Mentioned in release notes
- Credited in dataset-specific documentation

## 📞 Getting Help

Need help contributing?

- 💬 **Discussions:** [GitHub Discussions](https://github.com/SparkyScience/sparky-open-datasets/discussions)
- 🐛 **Issues:** [Report an issue](https://github.com/SparkyScience/sparky-open-datasets/issues)
- 📧 **Email:** [Contact maintainers]

## 📚 Additional Resources

- [GitHub Flow Guide](https://guides.github.com/introduction/flow/)
- [Markdown Guide](https://www.markdownguide.org/)
- [Creative Commons Licenses](https://creativecommons.org/licenses/)
- [GDPR Compliance](https://gdpr.eu/)

## 🙏 Thank You

Your contributions help advance open science and make valuable data accessible to researchers worldwide. We appreciate your time and effort!

---

**Questions?** Open an issue or start a discussion. We're here to help!
