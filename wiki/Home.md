<br />
<p align="center">
  <a href="https://github.com/palexdev/MaterialFX">
    <img src=https://imgur.com/7NdnoFl.png" alt="Logo">
  </a>
</p>

***

### Welcome to the MaterialFX wiki!

</div>

## Getting Started

### Requirements
- **Java 11 or higher** (JDK 17 recommended)
- **JavaFX 21+** (automatically handled by the library)

### Quick Start

Add MaterialFX to your project:

**Gradle:**
```groovy
dependencies {
    implementation 'io.github.palexdev:materialfx:11.17.0'
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

### Setting Up Themes

MaterialFX requires explicit theme initialization. Add this to your Application's `start()` method:

```java
import io.github.palexdev.materialfx.theming.JavaFXThemes;
import io.github.palexdev.materialfx.theming.MaterialFXStylesheets;
import io.github.palexdev.materialfx.theming.UserAgentBuilder;

public class MyApp extends Application {
    @Override
    public void start(Stage stage) {
        // Initialize MaterialFX theme
        UserAgentBuilder.builder()
            .themes(JavaFXThemes.MODENA)
            .themes(MaterialFXStylesheets.forAssemble(true))
            .setDeploy(true)
            .setResolveAssets(true)
            .build()
            .setGlobal();
        
        // Your scene setup here
        Scene scene = new Scene(root, 800, 600);
        stage.setScene(scene);
        stage.show();
    }
}
```

### First Component Example

```java
import io.github.palexdev.materialfx.controls.MFXButton;

// Create a MaterialFX button
MFXButton button = new MFXButton("Click Me!");
button.setOnAction(e -> System.out.println("Button clicked!"));
```

## Components

### Form Controls
* [Buttons](https://github.com/palexdev/MaterialFX/wiki/Buttons) - Material Design buttons with multiple variants
* [Check Boxes](https://github.com/palexdev/MaterialFX/wiki/Check%20Boxes.md) - Stylish checkboxes with smooth animations
* [Radio Buttons](https://github.com/palexdev/MaterialFX/wiki/Radio%20Button.md) - Material radio button controls
* [Text Fields](https://github.com/palexdev/MaterialFX/wiki/Text%20Fields.md) - Enhanced text input fields
* [Toggles](https://github.com/palexdev/MaterialFX/wiki/Toggles) - Modern toggle switches

### Selection Controls
* [Combo Boxes](https://github.com/palexdev/MaterialFX/wiki/Combo%20Boxes.md) - Redesigned dropdown selection controls
* [Date Pickers](https://github.com/palexdev/MaterialFX/wiki/Date%20Pickers.md) - Date selection components

### Display & Layout
* [Lists](https://github.com/palexdev/MaterialFX/wiki/Lists.md) - Virtualized list views for performance
* [Tables](https://github.com/palexdev/MaterialFX/wiki/Table%20Views.md) - Completely redesigned table views
* [Steppers](https://github.com/palexdev/MaterialFX/wiki/Stepper.md) - Multi-step form navigation
* [Paginations](https://github.com/palexdev/MaterialFX/wiki/Pagination.md) - Page navigation controls

### Feedback & Progress
* [Progress](https://github.com/palexdev/MaterialFX/wiki/Progress.md) - Progress bars and spinners
* [Tooltips](https://github.com/palexdev/MaterialFX/wiki/Tooltip.md) - Enhanced tooltip component

### Interactive Elements
* [Sliders](https://github.com/palexdev/MaterialFX/wiki/Slider.md) - Material Design sliders
* [Ripple Generators](https://github.com/palexdev/MaterialFX/wiki/Ripple%20Generator.md) - Material ripple effects

### Utilities
* [Filter Panes](https://github.com/palexdev/MaterialFX/wiki/Filter%20Pane.md) - Advanced filtering components

## Additional Resources

### Documentation
- **[Theming Guide](Theming-Guide.md)** - Complete guide to MaterialFX theming system
- **[Troubleshooting](Troubleshooting.md)** - Common issues and solutions
- **[Migration Guide](Migration-Guide.md)** - Migrating between versions or from JFoenix

### External Links
- **JavaDoc**: [javadoc.io/doc/io.github.palexdev/materialfx](https://javadoc.io/doc/io.github.palexdev/materialfx)
- **Demo Application**: Download the latest demo from [Releases](https://github.com/palexdev/MaterialFX/releases)
- **Source Code**: [GitHub Repository](https://github.com/palexdev/MaterialFX)
- **Report Issues**: [Issue Tracker](https://github.com/palexdev/MaterialFX/issues)
- **Discussions**: [GitHub Discussions](https://github.com/palexdev/MaterialFX/discussions)
- **Roadmap**: [Trello Board](https://trello.com/b/RqRwBIRh/materialfx-roadmap)

## Contributing

We welcome contributions! See the [CONTRIBUTING](https://github.com/palexdev/MaterialFX#contributing) section for guidelines.