# DuplicateFinder - Developer Documentation

This document provides technical information for developers who want to contribute to or understand the DuplicateFinder extension.

## Table of Contents

- [Project Overview](#project-overview)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Development Setup](#development-setup)
- [Building the Project](#building-the-project)
- [Architecture](#architecture)
- [Code Style Guidelines](#code-style-guidelines)
- [Testing](#testing)
- [Contributing](#contributing)

## Project Overview

DuplicateFinder is a Playnite extension plugin that helps users identify duplicate games in their library. The plugin integrates as a sidebar view in Playnite and provides both exact name matching and similarity-based detection using the Levenshtein distance algorithm.

**Key Features:**
- Exact duplicate detection by game name
- Similarity-based detection with configurable tolerance
- Filter option to include/exclude hidden games
- Sidebar UI integration with Playnite

## Technology Stack

- **Language**: C# (.NET Framework 4.6.2)
- **Build System**: MSBuild
- **UI Framework**: WPF (Windows Presentation Foundation)
- **Architecture Pattern**: MVVM (Model-View-ViewModel)
- **Dependencies**:
  - PlayniteSDK 6.11.0 - Playnite plugin SDK
  - Fastenshtein 1.0.10 - Levenshtein distance algorithm implementation

## Project Structure

```
DuplicateFinder/
├── DuplicateFinder.cs              # Main plugin class (GenericPlugin)
├── DuplicateFinderSidebarView.xaml # UI definition (View)
├── DuplicateFinderSidebarView.xaml.cs # View code-behind
├── DuplicateFinderSidebarViewModel.cs # ViewModel for sidebar
├── App.xaml                        # Application resources
├── extension.yaml                  # Plugin metadata
├── DuplicateFinder.yaml            # Additional plugin configuration
├── icon.png                        # Plugin icon
├── Localization/
│   ├── en_US.xaml                 # English localization
│   └── es_ES.xaml                 # Spanish localization
├── Properties/
│   └── AssemblyInfo.cs            # Assembly metadata
└── packages.config                 # NuGet package references
```

## Development Setup

### Prerequisites

1. **Visual Studio 2015 or later** (or any IDE supporting .NET Framework 4.6.2)
2. **Playnite** installed on your development machine
3. **.NET Framework 4.6.2 SDK** or later

### Getting Started

1. Clone the repository:
   ```bash
   git clone https://github.com/ap0ught/Playnite-DuplicateFinder.git
   cd Playnite-DuplicateFinder
   ```

2. Restore NuGet packages:
   ```bash
   dotnet restore DuplicateFinder.sln
   # or use Visual Studio's package restore
   ```

3. Open the solution:
   ```bash
   DuplicateFinder.sln
   ```

## Building the Project

### Using Command Line

```bash
# Debug build
dotnet build DuplicateFinder.sln --configuration Debug

# Release build
dotnet build DuplicateFinder.sln --configuration Release
```

### Using MSBuild

```bash
# Debug build
msbuild DuplicateFinder.sln /p:Configuration=Debug

# Release build
msbuild DuplicateFinder.sln /p:Configuration=Release
```

### Output

Build output is located in:
- Debug: `bin/Debug/`
- Release: `bin/Release/`

The main assembly is `DuplicateFinder.dll`.

### Installing for Testing

To test your changes in Playnite:

1. Build the project in Release mode
2. Copy the contents of `bin/Release/` to Playnite's extension directory:
   - Default location: `%AppData%\Playnite\Extensions\DuplicateFinder_d85c1809-6ab7-40c2-9434-af74feeae2d3\`
3. Restart Playnite to load the updated extension

## Architecture

### Design Pattern: MVVM

The project follows the Model-View-ViewModel pattern:

- **Model**: Plugin logic in `DuplicateFinder.cs`
- **View**: XAML UI in `DuplicateFinderSidebarView.xaml`
- **ViewModel**: `DuplicateFinderSidebarViewModel.cs`

### Main Components

#### 1. DuplicateFinder.cs

The main plugin class that inherits from `GenericPlugin`:

- **Lifecycle Management**: Initializes the plugin and integrates with Playnite
- **Core Logic**:
  - `FindDuplicates()`: Exact name matching
  - `FindSimilars()`: Similarity-based detection using Levenshtein distance
  - `ExecuteDuplicateFinder()`: Orchestrates the duplicate search
- **Event Handling**: Subscribes to ViewModel events

#### 2. DuplicateFinderSidebarViewModel.cs

ViewModel implementing `ObservableObject`:

- **Properties**: Bound to UI elements (FoundDuplicates, IncludeHiddenGames, CheckSimilarity, Tolerance)
- **Events**: `RequestFindDuplicates` event for communication with plugin
- **Commands**: Handles user actions from the UI

#### 3. DuplicateFinderSidebarView.xaml

WPF UserControl for the sidebar UI:

- Data binding to ViewModel properties
- UI controls for search options
- Results display with game information

### Key Algorithms

#### Levenshtein Distance

The similarity detection uses the Fastenshtein library for efficient Levenshtein distance calculation:

```csharp
Fastenshtein.Levenshtein levenshtein = new Fastenshtein.Levenshtein(gameName);
int distance = levenshtein.DistanceFrom(otherGameName);
```

The `tolerance` parameter determines the maximum allowed distance for games to be considered similar.

## Code Style Guidelines

**Critical**: Follow these guidelines strictly to maintain code consistency.

### Naming Conventions

- **Private fields**: camelCase with preceding underscore
  ```csharp
  private DuplicateFinderSidebarView _sidebarView;
  ```

- **Properties** (public and private): PascalCase
  ```csharp
  public bool IncludeHiddenGames { get; set; }
  ```

- **Methods** (public and private): PascalCase
  ```csharp
  private void ExecuteDuplicateFinder(bool includeHiddenGames, bool checkSimilarity, uint tolerance)
  ```

- **Do not expose public fields** - use properties instead

### Formatting Rules

1. **Indentation**: Use 4 spaces, NOT tabs
2. **Braces**: Allman style - opening brace `{` on a new line
3. **Empty lines**: Add empty line between `}` and next expression
4. **Always use braces**: Even for single-line statements

#### Correct Example

```csharp
if (includeHiddenGames)
{
    searchList = PlayniteApi.Database.Games;
}
else
{
    searchList = PlayniteApi.Database.Games.Where(game => !game.Hidden);
}

results = FindDuplicates(searchList);
```

#### Incorrect Example

```csharp
if (includeHiddenGames)
    searchList = PlayniteApi.Database.Games;
else
    searchList = PlayniteApi.Database.Games.Where(game => !game.Hidden);
results = FindDuplicates(searchList);
```

### Additional Guidelines

- Keep methods focused and single-purpose
- Use meaningful variable names
- Add comments only when necessary to explain complex logic
- Prefer LINQ for collection operations
- Use `var` when the type is obvious from the right side

## Testing

### Manual Testing

Currently, the project relies on manual testing within Playnite:

1. Build and deploy the extension to Playnite
2. Open Playnite and navigate to the sidebar
3. Test duplicate detection with various game libraries
4. Verify all features:
   - Exact name matching
   - Similarity detection
   - Hidden games filter
   - Tolerance adjustment

### Test Scenarios

- Games with identical names
- Games with similar names (sequels)
- Games with completely different names
- Mixed libraries (Steam, GOG, Epic, etc.)
- Libraries with/without hidden games

### Automated Tests

**Note**: No automated test infrastructure currently exists. Contributions to add unit tests are welcome.

## Contributing

### Before You Start

1. Open an issue to discuss the feature or fix
2. Wait for approval from maintainers
3. Fork the repository
4. Create a feature branch

### Development Workflow

1. Make your changes following the code style guidelines
2. Test thoroughly in Playnite
3. Commit with clear, descriptive messages
4. Push to your fork
5. Open a pull request

### Pull Request Guidelines

- Reference the related issue
- Describe what changed and why
- Include screenshots for UI changes
- Ensure code follows style guidelines
- Test with multiple game library configurations

### What to Contribute

**Good candidates for contribution:**
- Bug fixes
- Performance improvements
- UI/UX enhancements
- Additional localization languages
- Documentation improvements

**Discuss first:**
- New features
- Breaking changes
- Architecture changes

## Playnite SDK Resources

- [Playnite SDK Documentation](https://api.playnite.link/)
- [Playnite GitHub Repository](https://github.com/JosefNemec/Playnite)
- [Plugin Development Tutorials](https://playnite.link/docs/)

## License

This project is licensed under the MIT License. See [LICENSE.md](LICENSE.md) for details.

Copyright (c) 2024 Pablo García de los Salmones Gómez

## Third-Party Licenses

- [Fastenshtein](https://github.com/DanHarltey/Fastenshtein) - MIT License - Copyright (c) 2017 DanHartley
- [PlayniteSDK](https://playnite.link/) - MIT License - Copyright (c) 2020 Josef Nemec
