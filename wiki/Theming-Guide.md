# MaterialFX Theming Guide

## Overview

MaterialFX uses a sophisticated theming system based on the `UserAgentBuilder` that allows you to create custom, unified themes for your JavaFX applications.

## Why the New Theming System?

The traditional JavaFX `getUserAgentStylesheet()` approach has several limitations:
- Doesn't work well with nested custom controls
- User stylesheets are sometimes ignored
- Each control manages its own stylesheet separately

MaterialFX's new system solves these issues by:
- Combining multiple stylesheets into a single User-Agent stylesheet
- Providing consistent styling across all stages and scenes
- Supporting asset deployment for fonts and images
- Allowing integration with JavaFX's default themes

## Basic Theme Setup

### Minimal Setup

```java
import io.github.palexdev.materialfx.theming.JavaFXThemes;
import io.github.palexdev.materialfx.theming.MaterialFXStylesheets;
import io.github.palexdev.materialfx.theming.UserAgentBuilder;

@Override
public void start(Stage stage) {
    // Set up MaterialFX theme globally
    UserAgentBuilder.builder()
        .themes(JavaFXThemes.MODENA)  // Include JavaFX default theme
        .themes(MaterialFXStylesheets.forAssemble(true))  // MaterialFX theme + legacy
        .setDeploy(true)  // Deploy assets to disk
        .setResolveAssets(true)  // Resolve asset paths
        .build()
        .setGlobal();
    
    // Continue with scene setup...
}
```

### Without Legacy Controls

If you don't need legacy control support:

```java
UserAgentBuilder.builder()
    .themes(JavaFXThemes.MODENA)
    .themes(MaterialFXStylesheets.forAssemble(false))  // No legacy controls
    .setDeploy(true)
    .setResolveAssets(true)
    .build()
    .setGlobal();
```

## Advanced Theme Configuration

### Custom Themes

You can create custom theme entries by implementing the `Theme` interface:

```java
public class MyCustomTheme implements Theme {
    @Override
    public String path() {
        return "path/to/my/theme.css";
    }
    
    @Override
    public String load() {
        // Return the CSS content as a string
        return Files.readString(Path.of(path()));
    }
}
```

Then use it in the builder:

```java
UserAgentBuilder.builder()
    .themes(JavaFXThemes.MODENA)
    .themes(MaterialFXStylesheets.forAssemble(true))
    .themes(new MyCustomTheme())  // Add your custom theme
    .setDeploy(true)
    .setResolveAssets(true)
    .build()
    .setGlobal();
```

### Multiple Custom Stylesheets

```java
UserAgentBuilder.builder()
    .themes(JavaFXThemes.MODENA)
    .themes(MaterialFXStylesheets.forAssemble(true))
    .themes(
        MaterialFXStylesheets.BUTTON,
        MaterialFXStylesheets.CHECKBOX,
        MaterialFXStylesheets.TEXT_FIELD
    )
    .setDeploy(true)
    .setResolveAssets(true)
    .build()
    .setGlobal();
```

## Asset Management

### Why Deploy Assets?

When stylesheets are combined, relative paths to resources (fonts, images) break. MaterialFX solves this by:
1. Deploying assets to a temporary directory on disk
2. Resolving and updating paths in the combined stylesheet

### Asset Structure

Package your assets in a `.zip` file with proper structure:

```
assets.zip
├── fonts/
│   ├── MyFont.ttf
│   └── MyFont.css
└── images/
    └── icon.png
```

### Custom Deploy Location

Override the `deployName()` method in your Theme:

```java
@Override
public String deployName() {
    return "my-custom-theme";  // Assets will be at: temp-dir/my-custom-theme/
}
```

## Theme Components

### Available MaterialFX Stylesheets

All individual stylesheets are available via `MaterialFXStylesheets` enum:

- `BUTTON`
- `CHECKBOX`
- `COMBO_BOX`
- `DATE_PICKER`
- `DIALOG`
- `LIST_VIEW`
- `PROGRESS`
- `RADIO_BUTTON`
- `SCROLL_PANE`
- `SLIDER`
- `STEPPER`
- `TABLE_VIEW`
- `TEXT_FIELD`
- `TOGGLE_BUTTON`
- And many more...

### Using Individual Stylesheets

```java
UserAgentBuilder.builder()
    .themes(JavaFXThemes.MODENA)
    .themes(
        MaterialFXStylesheets.BUTTON,
        MaterialFXStylesheets.TEXT_FIELD
    )
    .build()
    .setGlobal();
```

## SceneBuilder Integration

MaterialFX controls automatically detect SceneBuilder and style themselves. To disable this:

```java
// In your main application before showing any UI
System.setProperty("materialfx.disableSceneBuilder", "true");
```

## Troubleshooting

### Styles Not Applying

1. **Check theme initialization**: Ensure `setGlobal()` is called before creating any scenes
2. **Verify asset deployment**: Check if `setDeploy(true)` is set
3. **Look for CSS errors**: Enable CSS debugging:
   ```java
   System.setProperty("javafx.css.debug", "true");
   ```

### Missing Fonts or Images

1. **Verify asset structure**: Ensure your assets are properly packaged
2. **Check asset resolution**: Set `setResolveAssets(true)`
3. **Review deployment logs**: Assets are deployed to system temp directory

### Performance Issues

1. **Reduce deployed assets**: Only include necessary resources
2. **Cache the builder**: Create the theme once and reuse it
3. **Profile initialization**: Most overhead occurs on first launch

## Best Practices

1. **Initialize once**: Call `UserAgentBuilder` once at application startup
2. **Include base theme**: Always include `JavaFXThemes.MODENA` for standard controls
3. **Deploy assets**: Use `setDeploy(true)` unless you have no custom resources
4. **Resolve assets**: Always use `setResolveAssets(true)` with deployed assets
5. **Test thoroughly**: CSS combining can be fragile; test all controls

## Migration from Old System

If you were using the deprecated `MFXThemeManager`:

**Old way:**
```java
MFXThemeManager.addOn(scene, Themes.DEFAULT, Themes.LEGACY);
```

**New way:**
```java
UserAgentBuilder.builder()
    .themes(JavaFXThemes.MODENA)
    .themes(MaterialFXStylesheets.forAssemble(true))
    .setDeploy(true)
    .setResolveAssets(true)
    .build()
    .setGlobal();
```

## Examples

### Complete Example Application

```java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;
import io.github.palexdev.materialfx.controls.MFXButton;
import io.github.palexdev.materialfx.theming.JavaFXThemes;
import io.github.palexdev.materialfx.theming.MaterialFXStylesheets;
import io.github.palexdev.materialfx.theming.UserAgentBuilder;

public class MaterialFXApp extends Application {
    
    @Override
    public void start(Stage stage) {
        // Initialize theme
        UserAgentBuilder.builder()
            .themes(JavaFXThemes.MODENA)
            .themes(MaterialFXStylesheets.forAssemble(true))
            .setDeploy(true)
            .setResolveAssets(true)
            .build()
            .setGlobal();
        
        // Create UI
        VBox root = new VBox(10);
        root.getChildren().add(new MFXButton("Material Button"));
        
        Scene scene = new Scene(root, 400, 300);
        stage.setScene(scene);
        stage.setTitle("MaterialFX Application");
        stage.show();
    }
    
    public static void main(String[] args) {
        launch(args);
    }
}
```

## Additional Resources

- [JavaFX CSS Reference Guide](https://openjfx.io/javadoc/21/javafx.graphics/javafx/scene/doc-files/cssref.html)
- [MaterialFX JavaDoc](https://javadoc.io/doc/io.github.palexdev/materialfx)
- [Material Design Guidelines](https://material.io/design)
