# Contributing to DMCA-Light

Thank you for your interest in contributing to DMCA-Light! This is an educational project focused on making semiconductor exciton physics accessible to students and researchers.

## Project Philosophy

DMCA-Light is designed to be:
- **Educational First**: Prioritize clarity and learning over performance optimization
- **Simple Physics**: Stick to the Wannier model (no advanced many-body physics)
- **Well-Tested**: Every feature should have comprehensive tests
- **Well-Documented**: Code should be self-explanatory with clear docstrings

## How to Contribute

### 1. Types of Contributions Welcome

**[+] New Materials**:
- Add semiconductors to the material database
- Include: name, formula, band_gap, epsilon, effective masses, lattice constant
- Must cite data sources (NREL, Materials Project, peer-reviewed papers)

**[!] Bug Fixes**:
- Fix calculation errors in Wannier model
- Correct API endpoint behavior
- Improve error handling

**[L] Documentation**:
- Improve physics explanations in README
- Add usage examples
- Clarify API documentation
- Fix typos or unclear instructions

**[T] Testing**:
- Add test cases for edge cases
- Improve test coverage
- Add validation tests for physics results

**[&] Infrastructure**:
- Improve CI/CD pipeline
- Optimize Docker configuration
- Enhance development tooling

### 2. What We DON'T Accept

**[-] Advanced Physics Models**:
- Ab initio DFT calculations (use DMCA Professional instead)
- Bethe-Salpeter equation (BSE) implementations
- Multi-particle effects (trions, biexcitons)
- Time-resolved dynamics

**Reason**: DMCA-Light is intentionally simplified for education. Advanced features belong in DMCA Professional.

**[-] Over-Engineering**:
- Complex abstractions for simple operations
- Premature optimization
- Unnecessary dependencies

**Reason**: Keep the codebase accessible to students learning Python/FastAPI.

## Development Workflow

### Step 1: Fork & Clone

```bash
# Fork the repository on GitHub
# Then clone your fork
git clone https://github.com/YOUR_USERNAME/DMCA-Light.git
cd DMCA-Light
```

### Step 2: Set Up Development Environment

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Install development dependencies
pip install pytest pytest-cov black flake8 mypy
```

### Step 3: Create a Branch

```bash
# Create feature branch
git checkout -b feature/your-feature-name

# Or bugfix branch
git checkout -b fix/issue-description
```

### Step 4: Make Your Changes

**Code Style**:
- Follow PEP 8 (enforced by `black` and `flake8`)
- Use type hints for function signatures
- Write clear docstrings (Google style)

**Example**:
```python
def calculate_reduced_mass(m_e: float, m_h: float) -> float:
    """Calculate reduced effective mass for exciton.

    Args:
        m_e: Electron effective mass (in units of m_0)
        m_h: Hole effective mass (in units of m_0)

    Returns:
        Reduced effective mass (in units of m_0)

    Example:
        >>> calculate_reduced_mass(0.067, 0.45)
        0.058
    """
    return (m_e * m_h) / (m_e + m_h)
```

### Step 5: Write Tests

**Test Requirements**:
- Unit tests for new functions
- Integration tests for new endpoints
- Edge case validation (negative values, zero epsilon, etc.)
- Physics validation (results in expected ranges)

**Example**:
```python
def test_calculate_exciton_gaas():
    """Test exciton calculation for GaAs."""
    material = {
        "name": "GaAs",
        "band_gap": 1.42,
        "epsilon": 12.9,
        "effective_mass_e": 0.067,
        "effective_mass_h": 0.45
    }

    result = calculate_exciton(material)

    # GaAs exciton: ~4-5 meV binding, ~10-12 nm radius
    assert 0.004 < result["binding_energy"] < 0.006
    assert 10 < result["bohr_radius"] < 12
```

### Step 6: Run Tests Locally

```bash
# Run all tests
pytest tests/ -v

# Check coverage
pytest tests/ --cov=app --cov-report=html

# Lint code
black app/ tests/
flake8 app/ tests/

# Type checking
mypy app/
```

**Quality Requirements**:
- Test coverage: >80%
- All tests passing
- No linting errors
- Type hints validated

### Step 7: Commit Your Changes

**Commit Message Format**:
```
<type>: <short description>

<detailed description>

<footer>
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `test`: Test additions/improvements
- `refactor`: Code refactoring
- `chore`: Build/CI changes

**Example**:
```bash
git add .
git commit -m "feat: Add InP material to database

Added Indium Phosphide (InP) with properties from NREL database:
- Band gap: 1.35 eV
- Epsilon: 12.4
- Effective masses from Vurgaftman et al. (2001)

Closes #42"
```

### Step 8: Push & Create Pull Request

```bash
# Push to your fork
git push origin feature/your-feature-name

# Create pull request on GitHub
# Fill in the PR template with:
# - What changed
# - Why it changed
# - How to test it
# - Related issues
```

## Pull Request Checklist

Before submitting, ensure:

- [ ] Code follows PEP 8 style guide
- [ ] All tests pass (`pytest tests/`)
- [ ] Test coverage >80% for new code
- [ ] Documentation updated (README, docstrings)
- [ ] CHANGELOG.md updated with your changes
- [ ] Commit messages are clear and descriptive
- [ ] No merge conflicts with `main` branch

## Code Review Process

1. **Automated Checks**: GitHub Actions will run tests, linting, security scans
2. **Maintainer Review**: A maintainer will review your code within 1-3 days
3. **Feedback Loop**: Address any requested changes
4. **Approval**: Once approved, maintainer will merge your PR
5. **Release**: Your changes will be included in the next release

## Material Data Sources

When adding new materials, use these trusted sources:

**Primary Sources**:
- [NREL Materials Database](https://www.nrel.gov/)
- [Materials Project](https://materialsproject.org/)
- [AFLOW](http://aflowlib.org/)

**Literature Sources**:
- Vurgaftman et al., "Band parameters for III-V compound semiconductors" (2001)
- Adachi, "Properties of Semiconductor Alloys" (2009)
- Landolt-Bornstein Database

**Citation Format**:
```python
# In material data or docstring
# Source: Vurgaftman et al., J. Appl. Phys. 89, 5815 (2001)
# Retrieved from: https://doi.org/10.1063/1.1368156
```

## Physics Validation Guidelines

All exciton calculations should be validated:

**Binding Energy Range**:
- Typical semiconductors: 1-50 meV
- GaAs: ~4.5 meV
- Si: ~14.7 meV
- GaN: ~25 meV

**Bohr Radius Range**:
- Typical semiconductors: 1-50 nm
- GaAs: ~11 nm
- Si: ~4.3 nm
- GaN: ~3.5 nm

**Red Flags**:
- Binding energy > 100 meV (likely calculation error)
- Bohr radius > 100 nm (unphysical for bulk)
- Negative values (input validation failure)

## Questions or Issues?

- **Bug Reports**: Open an issue with `[BUG]` prefix
- **Feature Requests**: Open an issue with `[FEATURE]` prefix
- **Questions**: Open a discussion in GitHub Discussions
- **Security Issues**: Email info@flamehaven.space (do not open public issue)

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

## Recognition

Contributors will be acknowledged in:
- CHANGELOG.md for each release
- README.md "Acknowledgments" section
- GitHub contributor graphs

Thank you for helping make semiconductor physics education more accessible!

---

**DMCA-Light** - Educational exciton physics for everyone.
