# Troubleshooting Guide

## Common Issues and Solutions

### Build and Setup Issues

#### Issue: "Cannot find MaterialFX dependency"

**Solution:**
Ensure Maven Central is in your repositories:

```groovy
repositories {
    mavenCentral()
}
```

#### Issue: "Module not found" error with Java modules

**Solution:**
Add MaterialFX to your `module-info.java`:

```java
module your.module {
    requires MaterialFX;
    requires javafx.controls;
    requires javafx.fxml;
}
```

#### Issue: Build fails with "Java 11 required"

**Solution:**
Verify your Java version:
```bash
java -version
```

Set the correct Java version in your build tool:

**Gradle:**
```groovy
sourceCompatibility = '11'
targetCompatibility = '11'
```

**Maven:**
```xml
<properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
</properties>
```

### Styling Issues

#### Issue: Controls appear unstyled

**Symptoms:**
- Controls look like default JavaFX components
- No Material Design styling visible

**Solutions:**

1. **Initialize the theme before creating any UI:**
```java
@Override
public void start(Stage stage) {
    // Theme MUST be initialized first
    UserAgentBuilder.builder()
        .themes(JavaFXThemes.MODENA)
        .themes(MaterialFXStylesheets.forAssemble(true))
        .setDeploy(true)
        .setResolveAssets(true)
        .build()
        .setGlobal();
    
    // Now create your scene
    Scene scene = new Scene(root, 800, 600);
    stage.setScene(scene);
    stage.show();
}
```

2. **Check for CSS loading errors:**
Enable CSS debugging:
```java
System.setProperty("javafx.css.debug", "true");
```

3. **Verify MaterialFX version:**
Ensure you're using version 11.14.0 or later (uses new theming system)

#### Issue: Custom stylesheets override MaterialFX styles

**Solution:**
Load custom styles after MaterialFX:

```java
scene.getStylesheets().add("path/to/custom.css");
```

Use more specific CSS selectors or `!important` (use sparingly):
```css
.mfx-button {
    -fx-background-color: blue !important;
}
```

#### Issue: Styles work in main scene but not in dialogs/popups

**Solution:**
MaterialFX uses global User-Agent stylesheet, so this should work automatically. If it doesn't:

1. Verify theme was set with `.setGlobal()`
2. Check for CSS specificity conflicts
3. Manually add stylesheets to the dialog scene if needed

### Runtime Issues

#### Issue: "JavaFX runtime components are missing"

**Symptoms:**
```
Error: JavaFX runtime components are missing, and are required to run this application
```

**Solutions:**

1. **For Java 11+**: Add JavaFX explicitly

**Maven:**
```xml
<dependencies>
    <dependency>
        <groupId>org.openjfx</groupId>
        <artifactId>javafx-controls</artifactId>
        <version>21</version>
    </dependency>
    <dependency>
        <groupId>org.openjfx</groupId>
        <artifactId>javafx-fxml</artifactId>
        <version>21</version>
    </dependency>
</dependencies>
```

**Gradle:**
```groovy
plugins {
    id 'org.openjfx.javafxplugin' version '0.0.13'
}

javafx {
    version = "21"
    modules = ['javafx.controls', 'javafx.fxml']
}
```

2. **Add VM arguments when running:**
```bash
--module-path /path/to/javafx-sdk/lib --add-modules javafx.controls,javafx.fxml
```

#### Issue: Application crashes with NullPointerException

**Common Causes:**

1. **Accessing UI from non-JavaFX thread**

**Wrong:**
```java
new Thread(() -> {
    button.setText("Updated"); // Crashes!
}).start();
```

**Correct:**
```java
new Thread(() -> {
    Platform.runLater(() -> {
        button.setText("Updated");
    });
}).start();
```

2. **Controls not initialized**
Always ensure controls are instantiated before use

3. **Missing FXML fx:id binding**
Check that your FXML fx:id matches your @FXML field name

#### Issue: High memory usage or memory leaks

**Solutions:**

1. **Remove listeners when disposing controls:**
```java
control.someProperty().removeListener(myListener);
```

2. **Clear large collections:**
```java
tableView.getItems().clear();
listView.getItems().clear();
```

3. **Use weak listeners for long-lived objects:**
```java
property.addListener(new WeakChangeListener<>(listener));
```

4. **Profile your application:**
Use Java Mission Control or VisualVM to identify leaks

### Component-Specific Issues

#### MFXTableView Issues

**Issue: Table doesn't update when data changes**

**Solution:**
Use observable collections:
```java
ObservableList<MyData> data = FXCollections.observableArrayList();
tableView.setItems(data);

// Updates will now be reflected
data.add(newItem);
```

**Issue: Poor performance with large datasets**

**Solution:**
MFXTableView uses virtualization by default. If still slow:
1. Reduce column computations
2. Use simple cell factories
3. Avoid complex bindings in cells

#### MFXComboBox Issues

**Issue: ComboBox doesn't show selected item**

**Solution:**
Ensure proper string converter or cell factory:
```java
comboBox.setConverter(new StringConverter<MyObject>() {
    @Override
    public String toString(MyObject object) {
        return object != null ? object.getName() : "";
    }
    
    @Override
    public MyObject fromString(String string) {
        // Implementation
        return null;
    }
});
```

#### MFXTextField Issues

**Issue: Text field doesn't accept input**

**Solutions:**
1. Check if field is editable: `textField.setEditable(true)`
2. Verify field is not disabled: `textField.setDisable(false)`
3. Ensure no key event handlers are consuming events

### SceneBuilder Issues

#### Issue: MaterialFX controls not appearing in SceneBuilder

**Solution:**

1. **Install the MaterialFX JAR in SceneBuilder:**
   - Build MaterialFX: `./gradlew build`
   - Find the JAR in `materialfx/build/libs/`
   - In SceneBuilder: JAR/FXML Manager → Add Library/FXML
   - Select the MaterialFX JAR

2. **For development:**
   - The build automatically copies to SceneBuilder library folder
   - Run: `./gradlew copyJar`

#### Issue: Controls appear unstyled in SceneBuilder

**Note:** This is expected behavior. MaterialFX controls should auto-style in SceneBuilder.

If they don't:
1. Restart SceneBuilder after adding the JAR
2. Check MaterialFX version (11.15.0+ has better support)
3. Verify the JAR is properly installed

### Performance Issues

#### Issue: Slow application startup

**Possible Causes:**

1. **Theme asset deployment overhead**
   - First launch deploys assets to temp directory
   - Subsequent launches are faster

2. **Large number of controls**
   - Use virtualization (MFXTableView, MFXListView)
   - Lazy load complex components

3. **Heavy initialization in Application.start()**
   - Move heavy operations to background threads
   - Show splash screen or loading indicator

**Optimization Example:**
```java
@Override
public void start(Stage stage) {
    // Show loading stage immediately
    Stage loadingStage = showLoadingScreen();
    
    // Initialize theme and heavy operations in background
    Task<Void> initTask = new Task<>() {
        @Override
        protected Void call() throws Exception {
            UserAgentBuilder.builder()
                .themes(JavaFXThemes.MODENA)
                .themes(MaterialFXStylesheets.forAssemble(true))
                .setDeploy(true)
                .setResolveAssets(true)
                .build()
                .setGlobal();
            return null;
        }
    };
    
    initTask.setOnSucceeded(e -> {
        loadingStage.close();
        showMainStage(stage);
    });
    
    new Thread(initTask).start();
}
```

#### Issue: Sluggish UI interactions

**Solutions:**

1. **Use virtualized controls for large datasets**
2. **Avoid expensive operations in event handlers**
3. **Use background threads for computations:**

```java
button.setOnAction(e -> {
    Task<Result> task = new Task<>() {
        @Override
        protected Result call() {
            return expensiveOperation();
        }
    };
    
    task.setOnSucceeded(event -> {
        Result result = task.getValue();
        updateUI(result);
    });
    
    new Thread(task).start();
});
```

### Debugging Tips

#### Enable Detailed Logging

```java
// JavaFX CSS debugging
System.setProperty("javafx.css.debug", "true");

// Pulse logging (rendering cycles)
System.setProperty("javafx.pulseLogger", "true");

// Scene graph logging
System.setProperty("prism.verbose", "true");
```

#### Visual Debugging Tools

1. **Scenic View** - Inspect JavaFX scene graph at runtime
   ```groovy
   implementation "io.github.palexdev:scenicview:17.0.2"
   ```

2. **CSS Debugger** - Use browser dev tools concepts
   - Enable with `-Djavafx.css.debug=true`

#### Common Debug Workflow

1. Enable CSS debugging
2. Check console for CSS parsing errors
3. Use Scenic View to inspect node hierarchy
4. Verify property values at runtime
5. Check for threading issues (use Platform.runLater)

## Getting Help

If you're still stuck after trying these solutions:

1. **Check existing issues:** [GitHub Issues](https://github.com/palexdev/MaterialFX/issues)
2. **Search discussions:** [GitHub Discussions](https://github.com/palexdev/MaterialFX/discussions)
3. **Read the JavaDoc:** [javadoc.io](https://javadoc.io/doc/io.github.palexdev/materialfx)
4. **Create a new issue:** Provide:
   - MaterialFX version
   - Java version
   - JavaFX version
   - Minimal reproducible example
   - Stack trace (if applicable)
   - Screenshots (for visual issues)

## Useful Commands

```bash
# Check Java version
java -version

# Check Gradle version
./gradlew --version

# Clean build
./gradlew clean build

# Run demo
./gradlew run

# View dependencies
./gradlew dependencies

# Build with verbose output
./gradlew build --info

# Build without tests
./gradlew build -x test
```
