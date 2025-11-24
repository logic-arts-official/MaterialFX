# Contributing to MaterialFX

Thank you for your interest in contributing to MaterialFX! This guide will help you get started.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [How to Contribute](#how-to-contribute)
- [Coding Standards](#coding-standards)
- [Submitting Changes](#submitting-changes)
- [Reporting Bugs](#reporting-bugs)
- [Requesting Features](#requesting-features)

## Code of Conduct

By participating in this project, you agree to maintain a respectful and inclusive environment for all contributors.

### Our Standards

- Use welcoming and inclusive language
- Be respectful of differing viewpoints and experiences
- Gracefully accept constructive criticism
- Focus on what is best for the community
- Show empathy towards other community members

## Getting Started

### Prerequisites

- Java 11 or higher (JDK 17 recommended for development)
- Git
- Gradle (wrapper included in project)
- A Java IDE (IntelliJ IDEA, Eclipse, or VS Code recommended)

### Quick Setup

1. **Fork the repository** on GitHub
2. **Clone your fork:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/MaterialFX.git
   cd MaterialFX
   ```
3. **Add upstream remote:**
   ```bash
   git remote add upstream https://github.com/palexdev/MaterialFX.git
   ```
4. **Build the project:**
   ```bash
   ./gradlew build
   ```
5. **Run the demo:**
   ```bash
   ./gradlew run
   ```

## Development Setup

### IDE Configuration

#### IntelliJ IDEA (Recommended)

1. Open IntelliJ IDEA
2. File → Open → Select MaterialFX directory
3. Wait for Gradle sync to complete
4. Mark `materialfx/src/main/java` as Sources Root
5. Mark `demo/src/main/java` as Sources Root

**Run Configurations:**
- Main class for demo: `io.github.palexdev.materialfx.demo.Demo`
- VM options: `-Dglass.disableGrab=true`

#### Eclipse

1. File → Import → Gradle → Existing Gradle Project
2. Select MaterialFX directory
3. Finish and wait for build

#### VS Code

1. Open MaterialFX folder
2. Install Java Extension Pack
3. Wait for Gradle sync
4. Use "Run" code lens in Demo.java

### Project Structure

```
MaterialFX/
├── materialfx/           # Main library code
│   ├── src/main/java/    # Java source files
│   ├── src/main/resources/ # CSS, images, fonts
│   └── build.gradle      # Library build config
├── demo/                 # Demo application
│   ├── src/main/java/    # Demo source files
│   └── build.gradle      # Demo build config
├── wiki/                 # Documentation
├── scripts/              # Build scripts
└── build.gradle          # Root build config
```

### Building and Testing

```bash
# Clean build
./gradlew clean build

# Build without tests
./gradlew build -x test

# Run tests only
./gradlew test

# Run specific test
./gradlew test --tests "ClassName.methodName"

# Build with verbose output
./gradlew build --info

# Generate JavaDoc
./gradlew javadoc
```

### Running the Demo

```bash
# Run default demo
./gradlew run

# Run specific demo (if multiple)
./gradlew run -PchooseMain=io.github.palexdev.materialfx.demo.Demo
```

## How to Contribute

### Types of Contributions

- **Bug fixes** - Fix existing issues
- **New features** - Add new components or functionality
- **Documentation** - Improve README, wiki, or JavaDoc
- **Tests** - Add or improve test coverage
- **Code quality** - Refactoring, optimization, cleanup

### Contribution Workflow

1. **Check existing issues** to see if your idea is already being worked on
2. **Create an issue** to discuss major changes before starting work
3. **Create a feature branch:**
   ```bash
   git checkout -b feature/my-new-feature
   ```
4. **Make your changes** following the coding standards
5. **Test thoroughly** - ensure no regressions
6. **Commit your changes** with clear messages:
   ```bash
   git commit -m "Add: New component MFXAwesomeButton"
   ```
7. **Push to your fork:**
   ```bash
   git push origin feature/my-new-feature
   ```
8. **Create a Pull Request** on GitHub

### Pull Request Guidelines

- **Title:** Clear and descriptive (e.g., "Fix: MFXButton ripple effect on disabled state")
- **Description:** 
  - What changes were made
  - Why the changes were necessary
  - Related issue numbers (#123)
  - Screenshots (for UI changes)
- **Tests:** Add tests for new functionality
- **Documentation:** Update relevant documentation
- **One feature per PR:** Keep PRs focused and manageable

## Coding Standards

### Code Style

MaterialFX follows standard Java conventions with some specifics:

#### Naming Conventions

```java
// Classes: PascalCase
public class MFXButton extends Button { }

// Interfaces: PascalCase
public interface Themable { }

// Methods: camelCase
public void setButtonType(ButtonType type) { }

// Variables: camelCase
private String buttonText;

// Constants: UPPER_SNAKE_CASE
public static final int DEFAULT_SIZE = 100;

// CSS classes: kebab-case
.mfx-button-raised { }
```

#### File Organization

```java
// 1. Package declaration
package io.github.palexdev.materialfx.controls;

// 2. Imports (organized)
import javafx.scene.control.Button;
import io.github.palexdev.materialfx.utils.NodeUtils;

// 3. JavaDoc
/**
 * MaterialFX implementation of a button.
 */
public class MFXButton extends Button {
    // 4. Constants
    private static final String STYLE_CLASS = "mfx-button";
    
    // 5. Fields
    private ObjectProperty<ButtonType> buttonType;
    
    // 6. Constructors
    public MFXButton() {
        this("");
    }
    
    public MFXButton(String text) {
        super(text);
        initialize();
    }
    
    // 7. Initialization methods
    private void initialize() {
        getStyleClass().add(STYLE_CLASS);
    }
    
    // 8. Public methods
    public void setButtonType(ButtonType type) {
        buttonTypeProperty().set(type);
    }
    
    // 9. Properties
    public ObjectProperty<ButtonType> buttonTypeProperty() {
        if (buttonType == null) {
            buttonType = new SimpleObjectProperty<>(this, "buttonType");
        }
        return buttonType;
    }
    
    // 10. Private/protected methods
    private void updateStyle() {
        // Implementation
    }
}
```

### JavaDoc Standards

All public APIs must have JavaDoc:

```java
/**
 * MaterialFX implementation of a button with Material Design styling.
 * <p>
 * This button supports multiple variants including raised, flat, and outlined.
 * It features ripple effects and customizable styling through CSS.
 * 
 * @see ButtonType
 * @since 11.0.0
 */
public class MFXButton extends Button {
    
    /**
     * Sets the button type which determines its visual style.
     * 
     * @param type the button type to set
     * @throws NullPointerException if type is null
     */
    public void setButtonType(ButtonType type) {
        Objects.requireNonNull(type, "Button type cannot be null");
        buttonTypeProperty().set(type);
    }
}
```

### CSS Guidelines

```css
/* Component-level styles */
.mfx-button {
    -fx-background-color: -mfx-button-bg;
    -fx-text-fill: -mfx-button-text;
    -fx-padding: 8 16;
}

/* Pseudo-classes */
.mfx-button:hover {
    -fx-background-color: derive(-mfx-button-bg, -10%);
}

.mfx-button:pressed {
    -fx-background-color: derive(-mfx-button-bg, -20%);
}

/* Variants using style classes */
.mfx-button.mfx-button-raised {
    -fx-effect: dropshadow(gaussian, rgba(0,0,0,0.3), 4, 0, 0, 2);
}

/* Use CSS variables for colors */
:root {
    -mfx-button-bg: #6200EE;
    -mfx-button-text: white;
}
```

### Testing Standards

```java
@Test
public void testButtonCreation() {
    MFXButton button = new MFXButton("Test");
    assertEquals("Test", button.getText());
    assertTrue(button.getStyleClass().contains("mfx-button"));
}

@Test
public void testButtonAction() {
    MFXButton button = new MFXButton();
    AtomicBoolean clicked = new AtomicBoolean(false);
    button.setOnAction(e -> clicked.set(true));
    button.fire();
    assertTrue(clicked.get());
}
```

## Submitting Changes

### Commit Message Format

Use clear, descriptive commit messages:

```
Type: Brief description (50 chars or less)

More detailed explanation if needed (wrap at 72 chars).
- Bullet points are okay
- Use present tense: "Add feature" not "Added feature"
- Reference issues: "Fixes #123"

Types:
- Add: New feature or functionality
- Fix: Bug fix
- Update: Changes to existing functionality
- Refactor: Code restructuring without behavior change
- Docs: Documentation changes
- Test: Adding or updating tests
- Style: Code style changes (formatting, etc.)
```

Examples:
```
Add: MFXColorPicker component with Material Design styling

Fix: MFXButton ripple effect not working on disabled buttons
Fixes #456

Update: Improve MFXTableView performance with virtualization

Docs: Add theming guide to wiki

Refactor: Simplify MFXTextField initialization logic

Test: Add unit tests for MFXCheckBox states
```

### Before Submitting

Checklist:
- [ ] Code compiles without errors
- [ ] All tests pass (`./gradlew test`)
- [ ] Code follows project style guidelines
- [ ] JavaDoc added for public APIs
- [ ] Documentation updated (if needed)
- [ ] No unnecessary dependencies added
- [ ] Git history is clean (no merge commits, good commit messages)

### Rebase Before Submitting

Keep your branch up-to-date:

```bash
git fetch upstream
git rebase upstream/main
```

If there are conflicts, resolve them and continue:

```bash
# Fix conflicts in files
git add .
git rebase --continue
```

## Reporting Bugs

### Before Reporting

1. Check if the issue already exists
2. Try to reproduce with the latest version
3. Test with the demo application

### Bug Report Template

```markdown
**Describe the bug**
A clear description of what the bug is.

**To Reproduce**
Steps to reproduce the behavior:
1. Create a MFXButton
2. Set property X to Y
3. Click the button
4. See error

**Expected behavior**
What you expected to happen.

**Actual behavior**
What actually happened.

**Screenshots**
If applicable, add screenshots.

**Environment:**
- MaterialFX version: [e.g., 11.17.0]
- Java version: [e.g., 17.0.5]
- JavaFX version: [e.g., 21]
- OS: [e.g., Windows 11, macOS 13]

**Code sample:**
```java
MFXButton button = new MFXButton("Test");
// Minimal code to reproduce
```

**Stack trace:**
```
Exception message and stack trace if applicable
```
```

## Requesting Features

### Feature Request Template

```markdown
**Is your feature request related to a problem?**
A clear description of the problem.

**Describe the solution you'd like**
What you want to happen.

**Describe alternatives you've considered**
Other approaches you've thought about.

**Additional context**
Screenshots, mockups, or links to Material Design guidelines.

**Would you be willing to implement this?**
Yes/No - If yes, we can discuss the approach!
```

## Community

- **GitHub Discussions:** Ask questions, share ideas
- **Issues:** Report bugs, request features
- **Discord/Slack:** (if available) Real-time chat

## Recognition

Contributors will be:
- Credited in release notes
- Listed in the project's contributors
- Given public acknowledgment

Thank you for contributing to MaterialFX! 🎉
