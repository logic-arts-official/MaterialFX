# TODOs and Future Plans

# IMPORTANT!

A more complete roadmap is now available at [Trello](https://trello.com/b/RqRwBIRh/materialfx-roadmap)

## Pros and Cons of MaterialFX

### Pros ✅

- **Material Design Compliant**: Components follow Google's Material Design guidelines, providing a modern and consistent UI
- **Comprehensive Component Library**: Offers restyled JavaFX controls plus unique components (Stepper, enhanced TableViews, ComboBoxes, etc.)
- **Rich Utilities**: Provides utility classes for JavaFX and Java (NodeUtils, ColorUtils, StringUtils, etc.)
- **Active Development**: Regular updates and bug fixes from an engaged maintainer
- **Well Documented**: Extensive JavaDoc documentation available at javadoc.io
- **Open Source**: LGPL v3 license allows use in commercial projects
- **Community Support**: Active discussions and issue tracking on GitHub
- **SceneBuilder Integration**: Components can be used directly in SceneBuilder
- **Flexible Theming**: New theming system using UserAgentBuilder for customizable styling

### Cons ⚠️

- **JavaFX Dependency**: Inherits all JavaFX limitations and quirks
- **Learning Curve**: New theming system requires understanding of CSS fragment assembly
- **Breaking Changes**: Major version updates may require code migration
- **Manual Theme Management**: Themes must be explicitly set by developers (not automatic)
- **Resource Deployment Overhead**: Asset deployment to disk may add initialization time
- **Limited Backwards Compatibility**: Newer versions may not support older JavaFX versions
- **SceneBuilder Integration Limitations**: Auto-styling in SceneBuilder can be fragile and error-prone

## Backlog and Implementation Status

### Completed Features ✓

- MFXButton (multiple variants)
- MFXCheckBox
- MFXRadioButton
- MFXToggleButton
- MFXComboBox (completely redesigned)
- MFXTextField and variants
- MFXPasswordField
- MFXListView (with virtualization)
- MFXTableView (completely redesigned with VirtualizedFX)
- MFXScrollPane
- MFXSlider
- MFXProgressBar / MFXProgressSpinner
- MFXDatePicker
- MFXStepper
- MFXPagination
- MFXContextMenu
- MFXTooltip
- MFXDialog and MFXStageDialog
- MFXNotifications system
- MFXFilterPane
- Ripple Effect Generator
- Virtual Flow and Virtualization framework
- Advanced Theming System with UserAgentBuilder

## Priority Legend

- **HIGH**: you can expect the feature in the next major version
- **LOW**: the feature will be implemented when I feel like it, or if the request is so high that it escalates to **
  HIGH** priority
- **TBD**: the idea is there, the feature will be implemented at some point in the future

Note that you can influence the ROADMAP priority in two ways:

1) There's a high request for the feature
2) You can sponsor the project with the $50 one time tier

### New Features

| Priority       | Feature                                          | Notes                                                                                                                                          |
| -------------- | ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| **[HIGH]**     | MFXColorPicker                                   |                                                                                                                                                |
| **[HIGH]**     | MFXToasts                                        |                                                                                                                                                |
| **[HIGH]**     | Screenshot Tool                                  |                                                                                                                                                |
| **[HIGH]**     | MFXTextArea                                      |                                                                                                                                                |
| **[HIGH/TBD]** | NavBar/Drawer/TabPane/SideMenu                   | The idea is definitely there. But I still have to figure out the exact differences between those controls, which to implement and how to do it |
| **[HIGH/TBD]** | Theme API and Dark Theme for MaterialFX controls | The idea is definitely there. But I still have to figure out the best way to implement it                                                      |
| **[LOW]**      | MFXCard                                          |                                                                                                                                                |
| **[LOW]**      | MFXCheckTableView                                |                                                                                                                                                |
| **[LOW]**      | MFXChipView                                      |                                                                                                                                                |
| **[LOW]**      | MFXDateTimePicker                                |                                                                                                                                                |
| **[LOW]**      | MFXRangeSlider                                   |                                                                                                                                                |
| **[LOW]**      | MFXTimePicker                                    |                                                                                                                                                |
| **[LOW]**      | MFXBadges                                        |                                                                                                                                                |
| **[LOW]**      | MFXAccordion                                     |                                                                                                                                                |
| **[TBD]**      | MFXCheckComboBox                                 |                                                                                                                                                |
| **[TBD]**      | MFXImageView                                     |                                                                                                                                                |
| **[TBD]**      | MFXHighlighter                                   |                                                                                                                                                |
| **[TBD]**      | MFXSplitButton                                   |                                                                                                                                                |
| **[TBD]**      | MFXProgressButton                                |                                                                                                                                                |
| **[TBD]**      | MFXWaveProgressBar                               |                                                                                                                                                |
| **[TBD]**      | MFXRate                                          |                                                                                                                                                |
| **[TDB]**      | Compare Slider                                   |                                                                                                                                                |

### Improvements

| Priority  | Feature                               |
| --------- | ------------------------------------- |
| **[LOW]** | Further improve the NotifcationSystem |
| **[LOW]** | Improve MFXSlider                     |
| **[TBD]** | Auto completion for text fields       |