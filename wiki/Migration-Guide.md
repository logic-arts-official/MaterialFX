# Migration Guide

## Overview

This guide helps you migrate between different versions of MaterialFX and from other JavaFX UI libraries.

## Table of Contents

- [Migrating from JFoenix](#migrating-from-jfoenix)
- [Migrating from MaterialFX 11.13.x and earlier](#migrating-from-materialfx-1113x-and-earlier)
- [Migrating to MaterialFX 11.14.0+ (New Theming System)](#migrating-to-materialfx-11140-new-theming-system)
- [Version-Specific Migration Notes](#version-specific-migration-notes)

## Migrating from JFoenix

MaterialFX was designed as a successor to JFoenix with many improvements. Here's how to migrate:

### Component Mapping

| JFoenix | MaterialFX | Notes |
|---------|------------|-------|
| JFXButton | MFXButton | Similar API, better ripple effects |
| JFXCheckBox | MFXCheckBox | Improved animations |
| JFXRadioButton | MFXRadioButton | More customization options |
| JFXTextField | MFXTextField | Better floating label implementation |
| JFXPasswordField | MFXPasswordField | Enhanced security features |
| JFXComboBox | MFXComboBox | Completely redesigned, not 1:1 compatible |
| JFXListView | MFXListView | Uses VirtualizedFX for better performance |
| JFXTableView | MFXTableView | Completely redesigned API |
| JFXToggleButton | MFXToggleButton | Similar functionality |
| JFXSlider | MFXSlider | Enhanced styling |
| JFXProgressBar | MFXProgressBar | Multiple variants available |
| JFXSpinner | MFXProgressSpinner | Multiple animation styles |
| JFXDialog | MFXDialog / MFXStageDialog | More flexible API |

### Migration Example: Button

**JFoenix:**
```java
import com.jfoenix.controls.JFXButton;

JFXButton button = new JFXButton("Click Me");
button.setButtonType(JFXButton.ButtonType.RAISED);
button.setStyle("-jfx-button-type: RAISED;");
```

**MaterialFX:**
```java
import io.github.palexdev.materialfx.controls.MFXButton;
import io.github.palexdev.materialfx.enums.ButtonType;

MFXButton button = new MFXButton("Click Me");
// Button style is controlled through CSS classes
button.getStyleClass().add("mfx-button-raised");
```

### Migration Example: TextField

**JFoenix:**
```java
JFXTextField textField = new JFXTextField();
textField.setPromptText("Enter text");
textField.setLabelFloat(true);
```

**MaterialFX:**
```java
MFXTextField textField = new MFXTextField();
textField.setPromptText("Enter text");
textField.setFloatingText("Label");
textField.setFloatMode(FloatMode.BORDER);
```

### Key Differences

1. **Theming:** JFoenix uses traditional CSS, MaterialFX uses UserAgentBuilder
2. **API Design:** MaterialFX has more properties and better customization
3. **Performance:** MaterialFX uses VirtualizedFX for better list/table performance
4. **Modularity:** MaterialFX is more modular and maintainable

## Migrating from MaterialFX 11.13.x and earlier

### Summary of Changes

The biggest change is the theming system. Controls no longer style themselves automatically.

### Before (11.13.x and earlier)

```java
public class MyApp extends Application {
    @Override
    public void start(Stage stage) {
        MFXButton button = new MFXButton("Click Me");
        // Button was automatically styled via getUserAgentStylesheet()
        
        Scene scene = new Scene(new VBox(button), 400, 300);
        stage.setScene(scene);
        stage.show();
    }
}
```

### After (11.14.0+)

```java
import io.github.palexdev.materialfx.theming.JavaFXThemes;
import io.github.palexdev.materialfx.theming.MaterialFXStylesheets;
import io.github.palexdev.materialfx.theming.UserAgentBuilder;

public class MyApp extends Application {
    @Override
    public void start(Stage stage) {
        // Initialize theme BEFORE creating any UI
        UserAgentBuilder.builder()
            .themes(JavaFXThemes.MODENA)
            .themes(MaterialFXStylesheets.forAssemble(true))
            .setDeploy(true)
            .setResolveAssets(true)
            .build()
            .setGlobal();
        
        MFXButton button = new MFXButton("Click Me");
        Scene scene = new Scene(new VBox(button), 400, 300);
        stage.setScene(scene);
        stage.show();
    }
}
```

## Migrating to MaterialFX 11.14.0+ (New Theming System)

### Step 1: Update Dependencies

**Gradle:**
```groovy
dependencies {
    implementation 'io.github.palexdev:materialfx:11.17.0'  // Use latest version
}
```

**Maven:**
```xml
<dependency>
    <groupId>io.github.palexdev</groupId>
    <artifactId>materialfx</artifactId>
    <version>11.17.0</version>
</dependency>
```

### Step 2: Remove Old Theme Manager Code

**Remove this:**
```java
import io.github.palexdev.materialfx.theming.MFXThemeManager;
import io.github.palexdev.materialfx.theming.base.Theme;

MFXThemeManager.addOn(scene, Themes.DEFAULT, Themes.LEGACY);
```

### Step 3: Add New Theme Initialization

**Add this in your Application.start() method BEFORE creating any UI:**

```java
import io.github.palexdev.materialfx.theming.JavaFXThemes;
import io.github.palexdev.materialfx.theming.MaterialFXStylesheets;
import io.github.palexdev.materialfx.theming.UserAgentBuilder;

UserAgentBuilder.builder()
    .themes(JavaFXThemes.MODENA)  // JavaFX default theme
    .themes(MaterialFXStylesheets.forAssemble(true))  // MaterialFX + legacy
    .setDeploy(true)  // Deploy assets
    .setResolveAssets(true)  // Resolve asset paths
    .build()
    .setGlobal();  // Set as global User-Agent stylesheet
```

### Step 4: Handle Multiple Stages/Dialogs

**Old way:** Had to add themes to each scene
```java
MFXThemeManager.addOn(dialogScene, Themes.DEFAULT);
```

**New way:** Global theme applies to all stages automatically
```java
// No action needed - theme is global
Stage dialog = new Stage();
Scene dialogScene = new Scene(root);
dialog.setScene(dialogScene);
// dialogScene automatically has MaterialFX styling
```

### Step 5: Update Custom Stylesheets

If you were adding custom stylesheets:

**Old way:**
```java
scene.getStylesheets().add("my-custom.css");
```

**New way (option 1 - still works):**
```java
scene.getStylesheets().add("my-custom.css");
```

**New way (option 2 - integrated):**
```java
// Create a custom Theme
public class MyTheme implements Theme {
    @Override
    public String path() {
        return getClass().getResource("my-custom.css").toExternalForm();
    }
}

// Add to UserAgentBuilder
UserAgentBuilder.builder()
    .themes(JavaFXThemes.MODENA)
    .themes(MaterialFXStylesheets.forAssemble(true))
    .themes(new MyTheme())  // Your custom theme
    .build()
    .setGlobal();
```

## Version-Specific Migration Notes

### 11.17.0 → Current

- No breaking changes
- Recommended to update dependencies (see gradle.properties)
- All themes and APIs remain compatible

### 11.16.0 → 11.17.0

- Minor API refinements
- Bug fixes for theming system
- Improved SceneBuilder support

### 11.15.0 → 11.16.0

- Enhanced SceneBuilder integration
- Better theme asset resolution
- Control improvements

### 11.14.0 → 11.15.0

- Introduction of SceneBuilder auto-styling
- Added `Themable` interface
- Theme system stabilization

### 11.13.x → 11.14.0 (MAJOR CHANGE)

**Breaking Changes:**
- Complete theming system overhaul
- `getUserAgentStylesheet()` removed from controls
- `MFXThemeManager` deprecated
- Manual theme initialization required

**Action Required:**
Follow the [migration steps above](#migrating-to-materialfx-11140-new-theming-system)

### Before 11.13.0

For migrations from very old versions:

1. First migrate to 11.13.x
2. Test thoroughly
3. Then migrate to 11.14.0+

## Common Migration Issues

### Issue: Controls Not Styled After Update

**Problem:** Updated to 11.14.0+ but controls look unstyled

**Solution:** Initialize theme with UserAgentBuilder as shown above

### Issue: Custom CSS Not Applying

**Problem:** Custom stylesheets worked before but not now

**Solution:** 
1. Ensure theme is initialized before adding custom CSS
2. Check CSS specificity - may need more specific selectors
3. Consider integrating custom CSS into UserAgentBuilder

### Issue: Dialogs/Popups Not Styled

**Problem:** Main window styled but dialogs are not

**Solution:** Use `.setGlobal()` instead of applying theme to individual scenes

### Issue: Performance Degradation

**Problem:** App slower after migration

**Solutions:**
1. First launch deploys assets - subsequent launches are faster
2. Consider using `setDeploy(false)` if you don't need asset deployment
3. Profile to identify specific bottlenecks

### Issue: Module-related Errors

**Problem:** Module not found errors after update

**Solution:** Update module-info.java:
```java
module your.module {
    requires MaterialFX;
    requires javafx.controls;
    requires javafx.fxml;
    requires io.github.palexdev.mfxcore;
    requires io.github.palexdev.virtualizedfx;
}
```

## Testing Your Migration

### Checklist

- [ ] All controls are properly styled
- [ ] Custom CSS still works as expected
- [ ] Dialogs and popups are styled correctly
- [ ] Application starts without errors
- [ ] No console warnings about CSS
- [ ] SceneBuilder integration works (if used)
- [ ] Performance is acceptable

### Test Each Component Type

Create a test scene with one of each control you use:

```java
VBox testRoot = new VBox(10);
testRoot.getChildren().addAll(
    new MFXButton("Test Button"),
    new MFXCheckBox("Test Checkbox"),
    new MFXTextField(),
    new MFXComboBox<>(),
    // ... other controls
);

Scene testScene = new Scene(testRoot, 600, 800);
Stage testStage = new Stage();
testStage.setScene(testScene);
testStage.show();
```

Verify each control appears correctly styled.

## Getting Help

If you encounter issues during migration:

1. Check [Troubleshooting Guide](Troubleshooting.md)
2. Search [GitHub Issues](https://github.com/palexdev/MaterialFX/issues)
3. Ask in [GitHub Discussions](https://github.com/palexdev/MaterialFX/discussions)
4. Review [CHANGELOG](https://github.com/palexdev/MaterialFX/blob/main/CHANGELOG.md)

## Additional Resources

- [Theming Guide](Theming-Guide.md) - Detailed theming documentation
- [Component Documentation](Home.md) - Individual component guides
- [JavaDoc](https://javadoc.io/doc/io.github.palexdev/materialfx) - API reference
- [Demo Application](https://github.com/palexdev/MaterialFX/releases) - Working examples
